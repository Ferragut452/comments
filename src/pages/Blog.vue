<template lang="pug">
  #blog
    .bg-header
    .article-wrap
      .container
        .article
          .article-header
            h1.title The Road to Home
            .date June 18, 2015
          article.article-content
            p Made last it seen went no just when of by. Occasional entreaties comparison me difficulty so themselves. At brother inquiry of offices without do my service. As particular to companions at sentiments. Weather however luckily enquire so certain do. Aware did stood was day under ask. Dearest affixed enquire on explain opinion he. Reached who the mrs joy offices pleased. Towards did colonel article any parties.
            .img-wrap
              img.article-img(src="./../assets/article.jpg", alt="article foto")
              a(href="href").img-hover
                p Photo by John Dou
            p Needed feebly dining oh talked wisdom oppose at. Applauded use attempted strangers now are middleton concluded had. It is tried no added purse shall no on truth. Pleased anxious or as in by viewing forbade minutes prevent. Too leave had those get being led weeks blind. Had men rose from down lady able. Its son him ferrars proceed six parlors. Her say projection age announcing decisively men. Few gay sir those green men timed downs widow chief. Prevailed remainder may propriety can and.  Now for manners use has company believe parlors. Least nor party who wrote while did. 
            p We diminution preference thoroughly if. Joy deal pain view much her time. Led young gay would now state. Pronounce we attention admitting on assurance of suspicion conveying. That his west quit had met till. Of advantage he attending household at do perceived. Middleton in objection discovery as agreeable. Edward thrown dining so he my around to.
            p Sense child do state to defer mr of forty. Become latter but nor abroad wisdom waited. Was delivered gentleman acuteness but daughters. In as of whole as match asked. Pleasure exertion put add entrance distance drawings. In equally matters showing greatly it as. Want name any wise are able park when. Saw vicinity judgment remember finished men throwing.
            p Too leave had those get being led weeks blind. Had men rose from down lady able. Its son him ferrars proceed six parlors. Her say projection age announcing decisively men.
          .article-info
            .author
              i.fas.fa-user
              p Post by #[span Will Smith]
            .date-time
              i.far.fa-clock
              p Posted #[span June 18, 2015] at #[span 21.14]
            .article-comments
              i.fas.fa-comment
              p #[span {{allComments.length}}] comments
          Comments(:comments="comments" @fetchComments="fetchComments" @fetchAll="fetchAll")
          .more(v-if="showMore")
            button.more-btn(@click="Step()") load more comments
</template>

<script>
import Comments from "../components/Comments";

export default {
  name: "Blog",
  components: {
    Comments
  },
  data() {
    return {
      comments: [],
      allComments: [],
      countComments: 5
    };
  },
  computed: {
    showMore() {
      if (
        this.comments.length >= this.countComments &&
        this.comments.length < this.allComments.length
      ) {
        return true;
      } else {
        return false;
      }
    }
  },
  methods: {
    fetchComments() {
      this.api
        .getData("comments", {
          params: {
            count: this.countComments
          }
        })
        .then(
          res => {
            this.comments = res.data;
          },
          err => {
            console.log(err);
          }
        );
    },
    fetchAll() {
      this.api
        .getData("comments", {
          params: {
            count: 100
          }
        })
        .then(
          res => {
            this.allComments = res.data;
          },
          err => {
            console.log(err);
          }
        );
    },
    Step() {
      this.countComments += 5;
      this.fetchComments();
    }
  },
  created() {
    this.fetchComments();
    this.fetchAll();
  }
};
</script>

<style lang="scss">
.bg-header {
  height: 400px;
  background-image: url("./../assets/bg.jpg");
  background-repeat: no-repeat;
  background-size: cover;
}
.article {
  text-align: justify;
  background-color: #fff;
  overflow: hidden;
  position: relative;
  top: -80px;
  box-shadow: 0px 2px 15px rgba(0, 0, 0, 0.1);
  .article-content p {
    margin-left: 80px;
    margin-right: 80px;
    margin-bottom: 30px;
    margin-top: 30px;
    line-height: 26px;
    &:first-child {
      margin-top: 15px;
    }
  }
  .img-wrap {
    float: left;
    margin-right: 15px;
    margin-bottom: 15px;
    border-right: 5px solid #f2f2f2;
    border-top: 5px solid #f2f2f2;
    border-bottom: 5px solid #f2f2f2;
    position: relative;
    display: flex;
    &:hover .img-hover {
      transform: scale(1);
    }
    .img-hover {
      color: #fff;
      height: 90%;
      width: 94%;
      position: absolute;
      left: 3%;
      top: 5%;
      background-color: rgba(#000, 0.5);
      display: flex;
      align-items: flex-end;
      justify-content: center;
      transform: scale(0);
      transition: all 0.2s ease-in-out;
      p {
        margin: 0;
        margin-bottom: 10px;
        font-size: 13px;
        font-style: italic;
        color: #f2f2f2;
        font-family: "Stag Book Italic";
      }
      &:before {
        content: "";
        height: 40px;
        width: 64px;
        position: absolute;
        left: 50%;
        top: 50%;
        transform: translate(-50%, -50%);
        background-image: url("./../assets/Eye.png");
        background-repeat: no-repeat;
      }
    }
  }
}

.article-header {
  margin-left: 80px;
  margin-right: 80px;
}

.title {
  margin-top: 45px;
  font-family: "Stag Semibold";
  border-bottom: 2px solid #f2f2f2;
  padding-bottom: 5px;
  margin-bottom: 11px;
  position: relative;
  font-size: 30px;
  &:after {
    content: "";
    height: 32px;
    width: 32px;
    position: absolute;
    left: -50px;
    top: 1px;
    background-image: url("./../assets/Star.png");
    background-repeat: no-repeat;
  }
}

.date {
  color: #b6b6b6;
  font-size: 15px;
}

.article-info {
  color: #b6b6b6;
  display: flex;
  margin-left: 80px;
  margin-right: 80px;
  border-top: 2px solid #f2f2f2;
  border-bottom: 2px solid #f2f2f2;
  font-size: 15px;
  > div {
    display: flex;
    align-items: center;
    margin-right: 50px;
    i {
      margin-right: 7px;
      margin-bottom: 2px;
    }
  }

  span {
    font-family: "Stag Medium";
  }
  .author i {
    font-size: 13px;
  }
}

.more {
  display: flex;
  justify-content: center;
  position: relative;
  margin-left: 80px;
  margin-right: 80px;
  margin-bottom: 50px;
  &:after {
    content: "";
    width: 100%;
    height: 2px;
    background-color: #f2f2f2;
    position: absolute;
    top: 20px;
    left: 0;
  }
}

.more-btn {
  padding: 8px 20px;
  background-color: #fff;
  color: #b6b6b6;
  border: 2px solid #f2f2f2;
  position: relative;
  z-index: 1;
  cursor: pointer;
  transition: all 0.2s ease-in-out;
  &:hover {
    background-color: #ffd800;
    color: #454545;
  }
}
</style>
