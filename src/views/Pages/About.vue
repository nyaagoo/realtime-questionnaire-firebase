<template lang="pug">
  .about
    h1 現在の質問
    h2 {{question.title}}
    .question-table
      .question-row-label
        .question-column-title タイトル
        .question-column 画像
        .question-column.label-yes Yes
        .question-column.label-no No
        .question-column アクション
      .question-row(v-for="q in questions" :key="q.title")
        .question-column-title {{q.title}}
        .question-column
          img(:src="q.img" height=24 width=24)
        .question-column.label-yes {{ q.yes }}
        .question-column.label-no {{ q.no }}
        .question-column
          button(@click="updateCurrent(q.id)") 更新
    .add-question-container
      input(v-model="title" placeholder="質問タイトル")
      input(v-model="imgUrl" placeholder="画像URL(省略可能)")
      button(@click="addQuestion()") 追加
</template>

<script lang="ts">
import { Component, Prop, Vue } from "vue-property-decorator";
import * as firebase from "firebase/app";

@Component
export default class HelloWorld extends Vue {
  currentQuestionId: string = "";
  title: string = "";
  imgUrl: string = "";
  question: {
    id: string;
    title: string;
    img: string;
    no: number;
    yes: number;
  } = {
    id: "",
    title: "",
    img: "",
    yes: 0,
    no: 0
  };
  questions: {
    id: string;
    title: string;
    img: string;
    no: number;
    yes: number;
  }[] = [];

  created() {
    firebase
      .firestore()
      .collection("questions")
      .doc("currentQuestion")
      .onSnapshot(doc => {
        this.currentQuestionId = doc.data()!.id;
        this.fetchCurrentQuestion();
      });

    firebase
      .firestore()
      .collection("votes")
      .onSnapshot(docs => {
        this.questions = [];
        docs.forEach(doc => {
          const question = {
            id: doc.id,
            title: doc.data()!.title,
            img: doc.data()!.img,
            yes: doc.data()!.yes,
            no: doc.data()!.no
          };
          this.questions.push(question);
        });
      });
  }

  fetchCurrentQuestion() {
    firebase
      .firestore()
      .collection("votes")
      .doc(this.currentQuestionId)
      .get()
      .then(doc => {
        const question = {
          id: doc.id,
          title: doc.data()!.title,
          img: doc.data()!.img,
          yes: doc.data()!.yes,
          no: doc.data()!.no
        };
        this.question = question;
      });
  }

  updateCurrent(docId: string) {
    firebase
      .firestore()
      .collection("questions")
      .doc("currentQuestion")
      .update({ id: docId });
  }

  addQuestion() {
    firebase
      .firestore()
      .collection("votes")
      .add({
        title: this.title,
        img:
          this.imgUrl ||
          "https://realtime-ias.web.app/mark_question.png",
        yes: 0,
        no: 0
      });
  }
}
</script>

<style scoped lang="stylus">
.about
  width 600px
  margin auto

.question-row-label
  display flex
  justify-content center
  margin 8px
  border-bottom 2px solid #cccccc

.question-row 
  display flex
  justify-content center
  margin 8px
  border-bottom 2px solid #cccccc

.question-column-title
  width: 240px;

.question-column 
  width 120px

.label-yes 
  color #0066B5
  font-size 18px

.label-no
  color #C9242A
  font-size 18px

</style>
