<template>
    <div class="container-fluid">
        <div class="row" id="head">

            <progressIndicator
                    :anzahlFragen="quiz.quizfragen.length"
                    :progress="loesung.history"
                    :selbsttest="quiz.selbsttest"
                    class="progressIndicator col-12"
            />

        </div>


        <div id="quizbody" v-show="!loading">
            <!--            Unterschiedliche Fragetypen-->
            <div v-if="started && !fertig">
                <multibleChoice
                        :geprueft="geprueft"
                        :quizfrage="quiz.quizfrage"
                        :selbsttest="quiz.selbsttest"
                        :solution="loesung.solution"
                        :started="started"
                        @newsel="aktualisiereSelected"
                        @timeover="pruefe"
                        ref="frage"
                        v-if="quiz.quizfrage.type === 'multible'"/>

                <Sorting
                        :geprueft="geprueft"
                        :quizfrage="quiz.quizfrage"
                        :selbsttest="quiz.selbsttest"
                        :solution="loesung.solution"
                        :started="started"
                        :index="aktuelleFrage"
                        :proFrage="loesung.history.proFrage"
                        @newsel="aktualisiereSelected"
                        @timeover="pruefe"
                        ref="frage"
                        v-else/>

            </div>

            <ControlButtons
                    :aktuelleFrage="aktuelleFrage" :anzahlFragen="quiz.quizfragen.length" :frage="quiz.quizfrage"
                    :geprueft="geprueft"
                    :selbsttest="quiz.selbsttest" @fertig="fertig=true"
                    @naechste="naechsteFrage"
                    @pruefe="pruefe"
                    @vorherige="vorherigeFrage"
                    v-if="started && !fertig"/>


            <!--        Startbereich-->
            <div v-if="!started">
                <h6 class="mb-2">Bitte Quiz eingeben</h6>
                <b-input class="mb-2" type="url" v-model="baseURL"></b-input>
                <b-button @click="naechsteFrage">Start</b-button>
            </div>

            <!--            Endbereich-->
            <div v-if="fertig">
                <h3>Fertig</h3>
            </div>
        </div>


        <div class="text-center spinner h-100" v-show="loading">
            <b-spinner type="grow"></b-spinner>
        </div>
    </div>
</template>

<script>
    /* eslint-disable no-console */

    import multibleChoice from "@/components/QuizView/Fragetypen/multibleChoice/multibleChoice";
    import ControlButtons from "@/components/QuizView/QuizSubIO/ControlButtons";
    import axios from "axios"
    import ProgressIndicator from "@/components/QuizView/QuizSubIO/progressIndicator";
    import Sorting from "@/components/QuizView/Fragetypen/Sorting/Sorting";

    export default {
        data() {
            return {
                baseURL: "https://quiz.educorvi.de/Members/julian--pollinger/testordner",
                loading: true,
                quiz: {
                    title: "",
                    //aktuelle frage
                    quizfrage: {
                        solutionimage: null,
                        solutionvideo: null,
                        erklaerung: ""
                    },
                    //alle Fragen
                    quizfragen: [],

                    selbsttest: true
                },

                loesung: {
                    //die Antworten des Users
                    selected: [[]],

                    //richtig und falsch der vergangenen Fragen
                    history: {
                        proFrage: [],
                        richtig: 0,
                        falsch: 0
                    },

                    solution: {}
                },

                aktuelleFrage: 0,

                started: false,
                fertig: false,

                geprueft: false,


                local: {
                    config: {
                        headers: {
                            Accept: "application/json"
                        }
                    }
                }

            }
        },
        name: "Quizview",
        components: {
            Sorting,
            ProgressIndicator,
            "multibleChoice": multibleChoice,
            ControlButtons
        },


        methods: {
            getQuizData(url) {
                this.loading = true;
                axios.get(url, this.local.config).then(res => {
                    this.quiz.quizfragen = res.data.items;
                    this.quiz.title = res.data.title;
                    this.loading = false;
                    this.deleteStuff()
                }).catch(err => this.fehler(err));
            },
            deleteStuff() {
                for (let i = 0; i < this.quiz.quizfragen.length; i++) {
                    if (this.quiz.quizfragen[i]["@type"] !== "Aufgabe") {
                        this.quiz.quizfragen.splice(i, 1);
                        i--;
                    }
                }
            },
            getFrage(url) {
                axios.get(url, this.local.config).then(this.contLoadingFrage).catch(err => this.fehler(err));
            },
            contLoadingFrage(res) {
                this.quiz.quizfrage = res.data;
                switch (this.quiz.quizfrage["antworten"][0]["bewertung"]) {
                    case "reihe":
                        this.quiz.quizfrage["type"] = "reihe";
                        break;
                    default:
                        this.quiz.quizfrage["type"] = "multible";
                }
                this.geprueft = false;
                this.$refs.frage.$refs.Countdown.startCountdown(this.quiz.quizfrage["bedenkzeit"]);
                this.$refs.frage.whipe();
                this.quiz.quizfrage.antworten = this.shuffle(this.quiz.quizfrage.antworten);
                this.loading = false;
            },
            aktualisiereSelected(sel) {
                this.loesung.selected[this.aktuelleFrage] = sel;
            },
            naechsteFrage() {
                this.loading = true;
                if (this.started) {
                    this.aktuelleFrage++;
                } else {
                    this.started = true;
                }
                if (this.loesung.selected[this.aktuelleFrage] === undefined) {
                    this.loesung.selected[this.aktuelleFrage] = [];
                }
                this.getFrage(Object.entries(this.quiz.quizfragen[this.aktuelleFrage])[0][1]);
            },
            vorherigeFrage() {
                this.loading = true;
                this.aktuelleFrage--;
                this.getFrage(Object.entries(this.quiz.quizfragen[this.aktuelleFrage])[0][1]);
            },
            pruefe() {
                let url = Object.entries(this.quiz.quizfragen[this.aktuelleFrage])[0][1] + "/validate-aufgabe-rest";
                if (this.quiz.quizfrage.type === "reihe") {
                    this.loesung.selected[this.aktuelleFrage] = this.quiz.quizfrage.antworten;
                }
                axios.get(url, {
                    headers: {
                        Accept: "application/json",
                    },
                    params: {
                        data: JSON.stringify(this.loesung.selected[this.aktuelleFrage])
                    }
                }).then(res => {
                    this.loesung.solution = res.data;
                    this.loesung.history.proFrage[this.aktuelleFrage] = this.loesung.solution.result;
                    this.geprueft = true;
                    this.setRichtigUndFalsch();
                    this.$refs.frage.$refs.Countdown.running = false;
                }).catch(err => this.fehler(err));

            },
            setRichtigUndFalsch() {
                this.loesung.history.richtig = 0;
                this.loesung.history.falsch = 0;
                for (let i = 0; i < this.loesung.history.proFrage.length; i++) {
                    let loe = this.loesung.history.proFrage[i];
                    if (loe) {
                        this.loesung.history.richtig++;
                    } else {
                        this.loesung.history.falsch++;
                    }
                }
            },
            fehler(err) {
                alert("Verbindung fehlgeschlagen\n(" + err + ")")
            },
            shuffle(a) {
                a.sort(() => Math.random() - 0.5);
                return a;
            }
        },

        created() {
            this.getQuizData(this.baseURL);
            // this.getFrage(this.quizfrag);
            // this.getFragen("http://192.168.86.48:8080/quiz/Members/julian--pollinger/testordner/");
        },
        props: {
        }
    }
</script>

<style scoped>
    #head {
        position: fixed;
        /* fixing the position takes it out of html flow - knows
                          nothing about where to locate itself except by browser
                          coordinates */
        left: 0; /* top left corner should start at leftmost spot */
        top: 0; /* top left corner should start at topmost spot */
        width: 100vw; /* take up the full browser width */
        z-index: 200; /* high z index so other content scrolls underneath */
        height: 26px;
        background: white;
        margin-left: 1px;
    }

    #quizbody {
        margin-top: 30px;
    }

    .spinner {
        margin-top: 50%;
    }


</style>