<template>
  <div id="app">
    <div class="moves">
      <div class="move-list">
        <h2 class="title">Выберите фильм</h2>
        <p v-for="(move, index) in moveList"
           :key="move.episode_id"
           @click="showedMoveContent = move.opening_crawl; activeMoveIndex = index"
           :class="{active: activeMoveIndex === index}"
           class="move-name"
        >
          {{ move.title }}
        </p>
      </div>
      <div class="move-content">
        <h2 class="title">Описание</h2>
        <p>{{showedMoveContent}}</p>
      </div>
    </div>
    <div class="feedback">
      <button v-if="!showForm && activeMoveIndex"
              @click="showForm = true"
      >
        Оставить отзыв
      </button>
      <form v-if="showForm" class="feedback-form" @submit.prevent="sendReview">
        <div class="form-inputs">
          <label for="username">Ваше имя</label>
          <input id="username" type="text" required v-model="review.username">
          <label for="email">Ваш email</label>
          <input id="email" type="email" required v-model="review.email">
          <label for="review">Ваш отзыв</label>
          <textarea id="review"
                    required
                    v-model="review.review_text"
                    rows="10"
                    maxlength="1000"
          ></textarea>
        </div>
        <div v-if="showForm" class="form-buttons">
          <button type="submit">Отправить</button>
          <button @click="showForm = false">Отмена</button>
        </div>
      </form>
    </div>
    <div class="modal" v-if="showModal">
      <div class="modal-content">
        <div class="modal-text">
          <p>{{review.username}}, ваш отзыв на фильм {{reviewData.chooseMove}} сохранен.</p>
          Ваш отзыв:
          <div class="modal-review">
            {{review.review_text}}
          </div>
        </div>
        <button @click="closeModal">Закрыть</button>
      </div>
    </div>
    <div class="waiting" v-if="showWaiting">
      <span>Ожидайте...</span>
    </div>
  </div>
</template>

<script>
export default {
  name: 'App',
  async mounted() {
   fetch('https://swapi.dev/api/films/')
   .then(respons => respons.json())
   .then(json => {
     console.log(json.results);
     this.rawMoveList = json.results;
   });
  },
  data() {
    return {
      rawMoveList: [],
      activeMoveIndex: '',
      showedMoveContent: '',
      showForm: false,
      showModal: false,
      showWaiting: false,
      review: {
        username: '',
        email: '',
        review_text: ''
      }
    }
  },
  methods: {
    sendReview: function () {
      const delay = new Promise((resolve) => { setTimeout(resolve, 1000) });
      this.showWaiting = true;
      delay.then(() => {
        this.showWaiting = false;
        this.showModal = true;
      });
    },
    closeModal: function () {
      this.showModal = false;
      for (let reviewKey in this.review) {
        this.review[reviewKey] = '';
      }
    }
  },
  computed: {
    moveList: function () {
      let moveList = [];
      this.rawMoveList.forEach(item => {
        const move = {
          title: '',
          opening_crawl: '',
          active: false
        };
        move.title = item.title;
        move.opening_crawl = item.opening_crawl;
        moveList.push(move);
      });
      return moveList;
    },
    reviewData: function () {
      const reviewData = {
        chooseMove: ''
      };
      if (this.activeMoveIndex) {
        reviewData.chooseMove = this.moveList[this.activeMoveIndex].title
      }
      return reviewData;
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Arial, sans-serif;
  .moves {
    display: flex;
    margin-bottom: 20px;
    .move-list {
      width: 50%;
      padding: 10px;
      .title {
        width: 100%;
        text-align: center;
      }
      .move-name {
        margin: 0;
        padding: 10px;
        cursor: pointer;
        &:hover {
          background-color: aqua;
        }
      }
      .active {
        background-color: blue;
      }
    }
    .move-content {
      width: 50%;
      background-color: lightgray;
      padding: 10px;
      .title {
        width: 100%;
        text-align: center;
      }
    }
  }
  .feedback {
    display: flex;
    flex-direction: column;
    align-items: center;
    padding: 10px;
    &-form {
      .form-inputs {
        display: flex;
        flex-direction: column;
        min-width: 300px;
        max-width: 300px;
        padding: 10px;
        label {
          margin-top: 10px;
        };
      }
      .form-buttons {
        display: flex;
        justify-content: center;
        button {
          margin-right: 10px;
          width: 100px;
        }
      }
    }
  }
  .modal {
    position: fixed;
    display: flex;
    justify-content: center;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    background-color: rgba(0,0,0,.5);
    &-content {
      display: flex;
      flex-direction: column;
      align-items: center;
      justify-content: space-between;
      box-sizing: border-box;
      background-color: white;
      margin-top: 50px;
      padding: 10px;
      width: 50%;
      min-width: 300px;
      max-width: 500px;
      height: 50%;
      min-height: 200px;
      max-height: 500px;
      .modal-text {
        width: 100%;
        .modal-review {
          padding: 10px;
          border: 1px solid black;
          border-radius: 5px;
          margin-bottom: 10px;
        }
      }
      button {
        width: 100px;
        justify-self: end;
      }
    }
  }
  .waiting {
    position: fixed;
    display: flex;
    justify-content: center;
    align-items: center;
    top: 0;
    bottom: 0;
    left: 0;
    right: 0;
    font-size: 50px;
  }
}
</style>
