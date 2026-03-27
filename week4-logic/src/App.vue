<script setup>
import { ref } from "vue";

const searchInput = ref("");
const searchKeyword = ref("");
const currentTab = ref("전체");

const isModalOpen = ref(false);
const selectedMovie = ref(null);

const movies = ref([
  {
    id: 1,
    title: "다크 나이트",
    genre: "액션",
    rating: 9.0,
    poster: "https://placehold.co/150x220/111111/FFFFFF?text=The+Dark+Knight",
  },
  {
    id: 2,
    title: "인터스텔라",
    genre: "SF",
    rating: 9.5,
    poster: "https://placehold.co/150x220/000000/FFFFFF?text=Interstellar",
  },
  {
    id: 3,
    title: "어바웃 타임",
    genre: "로맨스",
    rating: 8.8,
    poster: "https://placehold.co/150x220/FFB6C1/000000?text=About+Time",
  },
  {
    id: 4,
    title: "인셉션",
    genre: "액션",
    rating: 9.2,
    poster: "https://placehold.co/150x220/666666/FFFFFF?text=Inception",
  },
  {
    id: 5,
    title: "라라랜드",
    genre: "로맨스",
    rating: 6.9,
    poster: "https://placehold.co/150x220/F4A460/FFFFFF?text=LaLaLand",
  },
]);

const openModal = (movie) => {
  selectedMovie.value = movie;
  isModalOpen.value = true;
};

const searchMovie = () => {
  searchKeyword.value = searchInput.value;
};

const deleteMovie = (targetId) => {
  movies.value = movies.value.filter((movie) => movie.id !== targetId);
  isModalOpen.value = false;
  selectedMovie.value = null;
};
</script>

<template>
  <div class="container">
    <h1>영화 데이터베이스</h1>

    <div class="search-area">
      <input
        type="text"
        v-model="searchInput"
        @keyup.enter="searchMovie"
        placeholder="영화 제목 검색 후 엔터"
      />
      <button @click="searchMovie">검색</button>
    </div>

    <div class="tabs">
      <button
        @click="currentTab = '전체'"
        :class="{ active: currentTab === '전체' }"
      >
        전체
      </button>
      <button
        @click="currentTab = '액션'"
        :class="{ active: currentTab === '액션' }"
      >
        액션
      </button>
      <button
        @click="currentTab = 'SF'"
        :class="{ active: currentTab === 'SF' }"
      >
        SF
      </button>
      <button
        @click="currentTab = '로맨스'"
        :class="{ active: currentTab === '로맨스' }"
      >
        로맨스
      </button>
    </div>

    <div class="movie-grid">
      <div
        v-for="movie in movies"
        :key="movie.id"
        class="card"
        v-show="
          (currentTab === '전체' || movie.genre === currentTab) &&
          movie.title.includes(searchKeyword)
        "
      >
        <img :src="movie.poster" alt="포스터" />
        <h3>{{ movie.title }}</h3>
        <p>장르: {{ movie.genre }}</p>
        <p>평점: {{ movie.rating }}</p>

        <div class="badge-area">
          <span v-if="movie.rating >= 9" class="badge good">명작</span>
          <span v-else-if="movie.rating >= 7" class="badge soso">추천</span>
          <span v-else class="badge bad">킬링타임용</span>
        </div>

        <button class="detail-btn" @click="openModal(movie)">상세보기</button>
      </div>
    </div>

    <div v-if="isModalOpen" class="modal-bg" @click="isModalOpen = false">
      <div class="modal-content" @click.stop>
        <h2>{{ selectedMovie.title }} 상세정보</h2>
        <img :src="selectedMovie.poster" alt="포스터" />
        <p>장르: {{ selectedMovie.genre }}</p>
        <p>평점: {{ selectedMovie.rating }}</p>

        <div class="modal-actions">
          <button class="delete-btn" @click="deleteMovie(selectedMovie.id)">
            삭제하기
          </button>
          <button class="close-btn" @click="isModalOpen = false">닫기</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.container {
  max-width: 900px;
  margin: 0 auto;
  padding: 30px;
  text-align: center;
  font-family: sans-serif;
}

body {
  background-color: #ffffff;
  margin: 0;
}

#app {
  background-color: #ffffff;
  min-height: 100vh;
}

.search-area {
  margin: 20px 0;
}

.search-area input {
  padding: 10px;
  width: 220px;
  border: 1px solid #ccc;
  border-radius: 6px;
  margin-right: 8px;
  font-size: 15px;
}

.search-area button {
  padding: 10px 16px;
  border: none;
  background: black;
  color: white;
  border-radius: 6px;
  cursor: pointer;
  font-size: 15px;
}

.tabs {
  margin-bottom: 20px;
  display: flex;
  justify-content: center;
  gap: 10px;
}

.tabs button {
  padding: 8px 14px;
  border: none;
  background: #eeeeee;
  cursor: pointer;
  border-radius: 6px;
  transition: 0.2s;
  font-size: 15px;
}

.tabs button.active {
  background: #222222;
  color: white;
  font-weight: bold;
}

.movie-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 20px;
  justify-content: center;
}

.card {
  width: 180px;
  border: 1px solid #dddddd;
  padding: 15px;
  border-radius: 12px;
  text-align: center;
  box-shadow: 0 4px 10px rgba(0, 0, 0, 0.08);
  transition: 0.2s;
  background: white;
}

.card:hover {
  transform: translateY(-4px);
}

.card img {
  width: 100%;
  border-radius: 8px;
}

.badge-area {
  margin: 12px 0;
  font-weight: bold;
  font-size: 14px;
}

.badge {
  display: inline-block;
  padding: 6px 10px;
  border-radius: 20px;
}

.badge.good {
  background-color: #d1fae5;
  color: #065f46;
}

.badge.soso {
  background-color: #fef3c7;
  color: #92400e;
}

.badge.bad {
  background-color: #fee2e2;
  color: #991b1b;
}

.detail-btn {
  width: 100%;
  padding: 10px;
  border: none;
  background: #3b82f6;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
  margin-top: 8px;
}

.modal-bg {
  position: fixed;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  background: rgba(0, 0, 0, 0.6);
  display: flex;
  justify-content: center;
  align-items: center;
  z-index: 1000;
}

.modal-content {
  background: white;
  padding: 30px;
  border-radius: 12px;
  width: 320px;
  box-shadow: 0 8px 20px rgba(0, 0, 0, 0.25);
}

.modal-content img {
  width: 180px;
  border-radius: 8px;
  margin: 15px 0;
}

.modal-actions {
  display: flex;
  justify-content: space-between;
  gap: 10px;
  margin-top: 20px;
}

.delete-btn {
  flex: 1;
  padding: 10px;
  border: none;
  background: crimson;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}

.close-btn {
  flex: 1;
  padding: 10px;
  border: none;
  background: gray;
  color: white;
  border-radius: 8px;
  cursor: pointer;
  font-weight: bold;
}
</style>
