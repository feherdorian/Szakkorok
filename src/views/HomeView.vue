<template>
  <div class="container mt-5">
    <div class="row justify-content-center">
      <div v-for="(card, index) in cards" :key="index" class="col-md-3 mb-4">
        <div class="card text-center h-100" @click="goToPage(card.page)">
          <div class="card-body">
            <h3 class="card-title" v-html="card.title"></h3>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data(){
    return{

      cards: [
        { title: `Home`, page: "" },
        { title: `Szakkörök`, page: "Szakkor" },
      ],
    }
    },
  methods: {
    goToPage(page) {
      this.$router.push({ name: page });
    },
  },
};
</script>

<style scoped>
.card {
  cursor: pointer;
  transition: 0.3s ease;
  display: flex;
  flex-direction: column;
  min-height: 100px;
}

.card-body {
  display: flex;
  justify-content: center;
  align-items: center;
  flex-grow: 1;
  background: rgba(22, 22, 22, 0.2);
  padding: 5px;
}

.card:hover {
  transform: scale(1.05);
  box-shadow: 0px 4px 15px var(--text-color);
  animation: shaking 0.3s ease-in-out infinite;
}

.card-title {
  color: var(--text-color);
}

.black {
  color: #000;
}

@keyframes shaking {
  0% {
    transform: rotate(0deg);
  }
  25% {
    transform: rotate(2deg);
  }
  50% {
    transform: rotate(0deg);
  }
  75% {
    transform: rotate(-2deg);
  }
  100% {
    transform: rotate(0deg);
  }
}
</style>
