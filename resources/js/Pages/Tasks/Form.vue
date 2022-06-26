<template>
    <div>
        <header class="site-header">
            <div class="site-header__progress">
                <div
                    class="site-header__progress-bar"
                    :style="{ width: progress + '%' }"
                ></div>
            </div>
            <div class="container">
                <div class="site-header__content">
                    <div class="site-header__title">{{ title }}</div>
                    <div class="site-header__actions">
                        <button class="site-button">Nächste Frage</button>
                    </div>
                </div>
            </div>
        </header>
        <div class="container">
            <div class="site-content">
                <div class="site-notice">
                    Lorem ipsum dolor sit amet, consectetur adipiscing elit.
                    Mauris dui diam, auctor sed placerat id, egestas a purus.
                    Aliquam in orci sit amet magna mattis varius
                </div>
                <div class="test">
                    <div class="test__text">
                        <template
                            v-for="(text, i) in textForInsert"
                            :key="text"
                        >
                            <span
                                class="text__insert"
                                :class="{
                                    'text__insert--defined':
                                        orderAnswers[i - 1],
                                    'text__insert--focus':
                                        chosenAnswer === i - 1,
                                }"
                                v-if="i"
                                @drop="onDropAnswer($event, i - 1)"
                                @dragover.prevent
                                @dragenter.prevent
                                @click="chosenAnswer = i - 1"
                            >
                                <template v-if="orderAnswers[i - 1]">
                                    {{ getAnswer(i - 1) }}
                                    <span
                                        class="remove"
                                        @click="removeAnswer($event, i - 1)"
                                    ></span>
                                </template>
                            </span>
                            <span v-html="text"></span>
                        </template>
                    </div>
                    <div class="text__words words">
                        <div
                            class="words__wrap"
                            v-for="answer of answers"
                            :key="answer"
                            :id="answer.id"
                        >
                            <div
                                class="words__one"
                                :class="{
                                    disabled: isAnswerChosen(answer.id),
                                }"
                                draggable="true"
                                @dragstart="startDragAnswer($event, answer.id)"
                                @click="selectAnswer(answer.id)"
                            >
                                {{ answer.text }}
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>
<script>
export default {
    name: "InsertTest",
    /**
     * Test progress as a percentage
     */
    props: {
        progress: {
            type: Number,
            required: false,
            default: 20,
        },
    },
    data() {
        return {
            /**
             * Test title
             */
            title: "Some title",
            /**
             * Order of answers
             */
            orderAnswers: {},
            /**
             * User's chosen undefined answer
             */
            chosenAnswer: "",
            /**
             * Available answers data
             */
            answers: [
                {
                    id: 1,
                    text: "umgezogen",
                },
                {
                    id: 3,
                    text: "mich",
                },
                {
                    id: 4,
                    text: "einen",
                },
                {
                    id: 5,
                    text: "nächsten",
                },
                {
                    id: 6,
                    text: "für",
                },
                {
                    id: 7,
                    text: "lieber",
                },
                {
                    id: 8,
                    text: "wollen",
                },
                {
                    id: 9,
                    text: "Meines",
                },
            ],
            /**
             * Text for insert words
             */
            text: "Liebe Claudia,\n entschuldige, dass ich mich so lange nicht bei dir gemeldet\n Wie du weißt, bin ich vor zwei Wochen .... Deswegen hatte ich leider keine Zeit .... meine Freunde. Aber deinen Vorschlag, mal wieder gemeinsam .... Tag miteinander zu verbringen, finde ich sehr gut. Mir passt es auch am besten am Wochenende. Würde es bei dir schon .... Samstag gehen? Da habe ich noch nichts vor. Es würde .... natürlich freuen, wenn du dir bei dieser Gelegenheit auch meine neue Wohnung anschaust. Es ist wunderbar, so viel zu haben. Was meinst du, wir uns bei mir treffen und dann unsere Einkaufstour machen? Oder ist es dir .... , wenn wir zuerst einkaufen und am Abend zusammen kochen und essen? Übrigens, können wir dein Auto nehmen? .... steht schon wieder in der Werkstatt. Schreib mir doch bald, ob du an diesem Tag Zeit hast. .... du keine Zeit hast, finden wir sicher einen anderen Tag.\n Marion",
        };
    },
    computed: {
        /**
         * Divides text by separator and adds line breaks
         * @return {string[]}
         */
        textForInsert() {
            const text = this.text.split("....");
            return text.map((phrase) =>
                phrase.replace(/(?:\r\n|\r|\n)/g, "<br/>")
            );
        },
    },
    methods: {
        /**
         * Checks if an answer has been selected
         * @param answerId
         * @return {unknown}
         */
        isAnswerChosen(answerId) {
            return Object.values(this.orderAnswers).find(
                (id) => id == answerId
            );
        },
        /**
         * Action on start drag event. Set answer data to event
         * @param evt
         * @param id
         */
        startDragAnswer(evt, id) {
            evt.dataTransfer.dropEffect = "move";
            evt.dataTransfer.effectAllowed = "move";
            evt.dataTransfer.setData("answerID", id);
        },
        /**
         * Action on drop answer, set chosen answer
         * @param evt
         * @param insertIndex
         */
        onDropAnswer(evt, insertIndex) {
            const answerID = evt.dataTransfer.getData("answerID");
            this.chosenAnswer = insertIndex;
            this.selectAnswer(answerID);
        },
        /**
         * Get chosen answer text
         * @param i
         * @return {*|string}
         */
        getAnswer(i) {
            const answer = this.answers.find(
                (answer) => answer.id == this.orderAnswers[i]
            );
            return answer?.text || "";
        },
        /**
         * Set answer in orderAnswer
         * @param answerID
         */
        selectAnswer(answerID) {
            if (false !== this.chosenAnswer) {
                this.orderAnswers[this.chosenAnswer] = answerID;
                this.chosenAnswer = false;
            }
        },
        /**
         * Remove answer
         * @param e - event
         * @param i - index orderAnswers
         */
        removeAnswer(e, i) {
            e.stopPropagation();
            this.orderAnswers[i] = false;
        },
    },
};
</script>

<style scoped src="@/assets/pages/Tasks/Form.css"></style>
