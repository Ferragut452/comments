<template lang="pug">
  .comments
    h2.comments-title Leave comment:
    form.add-form(v-on:submit.prevent='addComment')
      .form-group.form-avatar
        .avatar
          img(src="./../assets/foto.jpg")
      .form-group.form-field
        textarea(name="text" v-model="newComment" placeholder="Your Message" required)
        button.btn(type='submit') Send
    ul.comment-list
      li.comment(v-for="(comment, index) in comments" :key="index")
        .comment-group
          .comment__avatar
            .avatar
              img(:src="comment.author.avatar", alt="avatar")
          .comment__body
            .info
              .name #[span.author-name {{comment.author.name}}] 
              .date-time #[i.far.fa-clock] #[span {{getDate(comment.created_at)}}] at #[span {{getTime(comment.created_at)}}]
            .form-group(v-if="comment.id == editCommentId")
              textarea(name="text" v-model="editingComment" placeholder="Your Message" required)
            p.content(v-else) {{comment.content}}
            ul.comment__controls
              li
                a(href="#" v-if="comment.id !== editCommentId" @click.prevent="toggleEditComment(comment.id, comment.content)")
                  i.far.fa-edit
                  span Edit
                a(href="#" v-else @click.prevent="saveEdit(comment.id)")
                  i.far.fa-save
                  span Save
              li
                a(href="#" @click.prevent="deleteComment(comment.id)")
                  i.fas.fa-times
                  span Delete
              li
                a(href="#" @click.prevent="openChildComment(comment.id)")
                  i.fas.fa-reply
                  span Reply
        form.add-form.reply(v-if="comment.id == openCommentId" v-on:submit.prevent='addChildComment(comment.id)')
          .form-group
            .top-reply
              .name
                i.fas.fa-share
                span {{comment.author.name}}
              a(href="#" @click.prevent="openCommentId = null").cancel
                i.fas.fa-times
                span Cancel
            textarea(rows="4" cols="45" name="text" v-model="newChildComment" placeholder="Your Message" required)
            button.btn(type='submit') Send
        ul.comment__children
          li.comment(v-for="(child, index) in comment.children" :key="index")
            .comment__avatar
              .avatar
                img(:src="child.author.avatar", alt="avatar")
            .comment__body
              .info
                .name #[span.author-name {{child.author.name}}] #[i.fas.fa-share] #[span.parent-name {{comment.author.name}}]
                .date-time #[i.far.fa-clock] #[span {{getDate(child.created_at)}}] at #[span {{getTime(child.created_at)}}]
              p.content {{child.content}}
</template>

<script>
export default {
  name: "Comments",
  props: ["comments"],
  data() {
    return {
      newComment: "",
      editingComment: "",
      newChildComment: "",
      editCommentId: null,
      openCommentId: null
    };
  },
  computed: {},
  methods: {
    getDate(el) {
      return el.slice(0, 10);
    },
    getTime(el) {
      return el.slice(11, 16);
    },
    addComment() {
      this.api.postData("comments", { content: this.newComment }).then(
        res => {
          this.$emit("fetchComments");
          this.$emit("fetchAll");
          this.newComment = "";
        },
        err => {
          console.log(err);
        }
      );
    },
    toggleEditComment(id, content) {
      this.editingComment = content;
      this.editCommentId = id;
    },
    saveEdit(id) {
      this.api.putData("comments/" + id, { content: this.editingComment }).then(
        res => {
          this.$emit("fetchComments");
          this.editCommentId = null;
        },
        err => {
          console.log(err);
        }
      );
    },
    addChildComment(id) {
      this.api
        .postData("comments", { content: this.newChildComment, parent: id })
        .then(
          res => {
            this.$emit("fetchComments");
            this.newChildComment = "";
            this.openCommentId = null;
          },
          err => {
            console.log(err);
          }
        );
    },
    deleteComment(id) {
      this.api.deleteData("comments/" + id).then(
        res => {
          this.$emit("fetchComments");
          this.$emit("fetchAll");
        },
        err => {
          console.log(err);
        }
      );
    },
    openChildComment(id) {
      this.newChildComment = "";

      if (this.openCommentId !== id) {
        this.openCommentId = id;
      } else {
        this.openCommentId = null;
      }
    }
  },
  created() {}
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
textarea::-webkit-input-placeholder {
  color: #bfbfbf;
}

textarea:-moz-placeholder {
  /* Firefox 18- */
  color: #bfbfbf;
}

textarea::-moz-placeholder {
  /* Firefox 19+ */
  color: #bfbfbf;
}

textarea:-ms-input-placeholder {
  color: #bfbfbf;
}

textarea::placeholder {
  color: #bfbfbf;
}

.comments {
  padding-left: 80px;
  padding-right: 80px;
  margin-bottom: 50px;
  &-title {
    font-family: "Stag Medium";
    font-weight: normal;
    font-size: 30px;
    margin-bottom: 40px;
    margin-top: 40px;
  }
}

.add-form {
  display: flex;
  &.reply {
    margin-left: 110px;
    .form-group {
      display: flex;
      flex-direction: column;
      margin-bottom: 0;
      margin-top: 30px;
      .btn {
        align-self: flex-end;
        margin-top: 20px;
      }
    }
    .top-reply {
      display: flex;
      justify-content: space-between;
      color: #b6b6b6;
      font-size: 15px;
      margin-bottom: 15px;
      i {
        margin-right: 5px;
      }
      a {
        font-size: 15px;
        color: #b6b6b6;
        &:hover {
          color: #929292;
        }
      }
    }
  }
}

.form-avatar,
.comment__avatar {
  .avatar {
    width: 90px;
    height: 90px;
    border: 2px solid #f2f2f2;
    border-radius: 5px;
    box-sizing: border-box;
    img {
      height: 100%;
      border-radius: 7px;
      padding: 2px;
      box-sizing: border-box;
    }
  }
}

.form-field {
  display: flex;
  flex-direction: column;
  flex: 1;
  margin-left: 20px;
  textarea {
    height: 90px;
    box-sizing: border-box;
    border-radius: 5px;
    padding: 10px 15px;
    border: 2px solid #f2f2f2;
  }
  .btn {
    align-self: flex-end;
    margin-bottom: 10px;
    margin-top: 10px;
  }
}

.comment-list {
  display: flex;
  flex-direction: column;
  margin-top: 0;
  border-top: 2px solid #f2f2f2;
}

.comment {
  margin-top: 40px;
  display: flex;
  flex-direction: column;
  .form-group {
    width: 100%;
    margin-top: 20px;
    margin-bottom: 20px;
  }
  textarea {
    width: 100%;
    box-sizing: border-box;
    border-radius: 5px;
    padding: 10px 15px;
    border: 2px solid #f2f2f2;
  }
  p {
    margin-top: 0;
    margin-bottom: 0;
  }
  &-group {
    display: flex;
  }
  &__body {
    flex: 1;
    display: flex;
    flex-direction: column;
    justify-content: space-between;
    .info {
      display: flex;

      .author-name {
        font-family: "Stag Medium";
        margin-right: 10px;
      }
      .parent-name {
        color: #b6b6b6;
        font-size: 15px;
        margin-right: 10px;
      }
      .fa-share,
      .fa-clock {
        color: #b6b6b6;
        font-size: 14px;
        margin-right: 3px;
      }

      .date-time {
        color: #b6b6b6;
        font-size: 15px;
        margin-top: 2px;
        span {
          font-family: "Stag Medium";
        }
      }
    }
    .content {
      margin-top: 15px;
      margin-bottom: 15px;
    }
  }
  &__children {
    margin-left: 90px;
    .comment {
      flex-direction: row;
      margin-left: 20px;
      margin-top: 30px;
      &__body {
        justify-content: flex-start;
        box-sizing: border-box;
        .content {
          margin-top: 7px;
        }
      }
    }
    .comment__avatar {
      .avatar {
        width: 70px;
        height: 70px;
        border: 2px solid #f2f2f2;
        border-radius: 5px;
        box-sizing: border-box;
        img {
          height: 100%;
          border-radius: 7px;
          padding: 2px;
          box-sizing: border-box;
        }
      }
    }
  }
  &__body {
    margin-left: 20px;
  }

  &__controls {
    width: 190px;
    display: flex;
    justify-content: space-between;
    li:first-child {
      width: 49px;
    }
    a {
      font-size: 15px;
      color: #b6b6b6;
      transition: all 0.2s ease-in-out;
      &:hover {
        color: #929292;
      }
    }
    span {
      padding-left: 4px;
    }
  }
}
</style>
