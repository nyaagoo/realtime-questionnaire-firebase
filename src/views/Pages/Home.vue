<template lang="pug">
  .home
    .question
      p.question-title Question
      p.question-content {{question.title}}
      .img-container
        img(alt="logo" :src="question.img" width=128 height=128)
      .results
        .result-yes
          span 
            b.label-yes YES
          .result-box-yes(:style="{ 'width': `${resultMaxWidth * result.yesRatio / 100}px` }")
            span.label-ratio {{ Math.round(result.yesRatio) }} %
        .result-no
          span
            b.label-no No
          .result-box-no(:style="{ 'width': `${resultMaxWidth * result.noRatio / 100}px` }")
            span.label-ratio {{  Math.round(result.noRatio) }} %
      button.button.button-yes(@click="voteYes()") Yes
      button.button.button-no(@click="voteNo()") No
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import * as firebase from "firebase/app";
import { TweenMax } from "gsap";

@Component({
  components: {}
})
export default class Home extends Vue {
  increment = firebase.firestore.FieldValue.increment(1);
  decrement = firebase.firestore.FieldValue.increment(-1);
  currentQuestionId: string = "";
  question: { title: string; img: string; no: number; yes: number } = {
    title: "",
    img: "",
    yes: 0,
    no: 0
  };
  result: { yes: number; no: number; yesRatio: number; noRatio: number } = {
    yes: 0,
    no: 0,
    yesRatio: 0,
    noRatio: 0
  };
  resultMaxWidth = 500;
  created() {
    firebase
      .firestore()
      .collection("questions")
      .doc("currentQuestion")
      .onSnapshot(doc => {
        this.currentQuestionId = doc.data()!.id;
        this.createSnapshot();
      });
  }

  createSnapshot() {
    firebase
      .firestore()
      .collection("votes")
      .doc(this.currentQuestionId)
      .onSnapshot(doc => {
        const yesCount = doc.data()!.yes;
        const noCount = doc.data()!.no;
        const totalCount = yesCount + noCount;
        const question = {
          title: doc.data()!.title,
          img: doc.data()!.img,
          yes: yesCount,
          no: noCount
        };
        this.question = question;
        const result = {
          yes: yesCount,
          no: noCount,
          yesRatio: !!totalCount ? Math.round((yesCount / totalCount) * 100) : 0,
          noRatio: !!totalCount ? Math.round((noCount / totalCount) * 100) : 0
        };
        TweenMax.to(this.result, 2, result);
      });
  }

  voteYes() {
    firebase
      .firestore()
      .collection("votes")
      .doc(this.currentQuestionId)
      .update({ yes: this.increment });
  }

  voteNo() {
    firebase
      .firestore()
      .collection("votes")
      .doc(this.currentQuestionId)
      .update({ no: this.increment });
  }
}
</script>

<style scoped lang="stylus">
.home
  width 600px
  margin auto

.question-title
  font-size 30px
  color #999

.question-content
  font-size 20px

.results
  margin 16px
  border-left 4px solid #ccc
  span
    font-size 30px

.result-yes, .result-no
  padding-top 24px

.result-box-yes
  height 64px
  width 300px
  border-top-right-radius 10px
  border-bottom-right-radius 10px
  background #0066B5

.result-box-no
  height 64px
  width 300px
  border-top-right-radius 10px
  border-bottom-right-radius 10px
  background #C9242A

.label-yes, .label-no
  padding 8px

.label-yes
  color #0066B5

.label-no
  color #C9242A

.label-ratio
  line-height 2
  color #fff

.button
  border 2px solid #88ff88
  border-radius 10px
  margin 20px
  background: #fff
  cursor pointer
  width 200px
  height 48px
  font-size 30px

.button-yes
  color #0066B5
  border 2px solid #0066B5

.button-no
  color #C9242A
  border 2px solid #C9242A
</style>
