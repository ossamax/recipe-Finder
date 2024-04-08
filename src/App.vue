<script setup>
import axios from "axios";
import { onMounted, ref, watchEffect } from "vue";
// import { data } from "../data";

const inputValue = ref("");
const data = ref([]);
function GetData() {
  axios
    .get("https://api.spoonacular.com/recipes/findByIngredients", {
      params: {
        ingredients: inputValue.value ? inputValue.value : "chicken,pasta,drinks",
        apiKey: "17cbab47fdf64d43b4e9f7661dd7ea66",
      },
    })
    .then((response) => {
      console.log(response.data);
      data.value = response.data;
    })
    .catch((error) => {
      this.error = error; // Store error in error variable
      console.error("Error fetching recipes:", error);
      this.loading = false; // Set loading state to false
    });
}

onMounted(GetData);
watchEffect(() => {
  GetData();
}, [inputValue]);
</script>

<template>
  <main>
    <div class="navigation">
      <nav>
        <a href="#">Home</a>
        <a href="#">Explore</a>
        <a href="#">Community</a>
        <a href="#">Favorites</a>
        <a href="#">Help</a>
        <a href="#">Settings</a>
      </nav>
    </div>
    <div class="wrapper">
      <header class="search_header">
        <div class="input_wrp">
          <span
            ><svg
              xmlns="http://www.w3.org/2000/svg"
              width="25"
              height="25"
              viewBox="0 0 24 24"
            >
              <path
                fill="currentColor"
                d="M15.5 14h-.79l-.28-.27a6.5 6.5 0 0 0 1.48-5.34c-.47-2.78-2.79-5-5.59-5.34a6.505 6.505 0 0 0-7.27 7.27c.34 2.8 2.56 5.12 5.34 5.59a6.5 6.5 0 0 0 5.34-1.48l.27.28v.79l4.25 4.25c.41.41 1.08.41 1.49 0c.41-.41.41-1.08 0-1.49zm-6 0C7.01 14 5 11.99 5 9.5S7.01 5 9.5 5S14 7.01 14 9.5S11.99 14 9.5 14"
              /></svg
          ></span>
          <input
            type="search"
            name="search"
            id="search"
            v-model="inputValue"
            placeholder="What do you want to cook today?"
          />
        </div>
        <button>
          <svg
            xmlns="http://www.w3.org/2000/svg"
            width="25"
            height="25"
            viewBox="0 0 24 24"
          >
            <g fill="none" stroke="currentColor" stroke-width="1.5">
              <rect width="18.5" height="18.5" x="2.75" y="2.75" rx="6" />
              <path
                stroke-linecap="round"
                stroke-linejoin="round"
                d="M17.75 8.805h-1.437m-3.594 0H6.25m11.5 6.39h-6.469m-3.593 0H6.25m3.234 1.797a1.797 1.797 0 1 0 0-3.594a1.797 1.797 0 0 0 0 3.594m5.032-6.39a1.797 1.797 0 1 0 0-3.594a1.797 1.797 0 0 0 0 3.594"
              />
            </g>
          </svg>
        </button>
      </header>
      <div class="intro">
        <div class="intro_head">
          <h1>Recommended Recipes</h1>
          <p>Base on your preferences</p>
        </div>
        <div class="pagination">
          <button>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
            >
              <path
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m14 7l-5 5l5 5"
              />
            </svg>
          </button>
          <button>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
            >
              <path
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m10 17l5-5l-5-5"
              />
            </svg>
          </button>
        </div>
      </div>
      <ul>
        <li v-for="(item, index) in data" :key="index">
          <div class="likes-icon">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="15"
              height="15"
              viewBox="0 0 256 256"
            >
              <path
                fill="currentColor"
                d="M178 36c-20.09 0-37.92 7.93-50 21.56C115.92 43.93 98.09 36 78 36a66.08 66.08 0 0 0-66 66c0 72.34 105.81 130.14 110.31 132.57a12 12 0 0 0 11.38 0C138.19 232.14 244 174.34 244 102a66.08 66.08 0 0 0-66-66m-5.49 142.36a328.69 328.69 0 0 1-44.51 31.8a328.69 328.69 0 0 1-44.51-31.8C61.82 159.77 36 131.42 36 102a42 42 0 0 1 42-42c17.8 0 32.7 9.4 38.89 24.54a12 12 0 0 0 22.22 0C145.3 69.4 160.2 60 178 60a42 42 0 0 1 42 42c0 29.42-25.82 57.77-47.49 76.36"
              /></svg
            >{{ item.likes }}
          </div>
          <div class="img-bg">
            <img :src="item.image" alt="image" class="card_img" />
          </div>
          <div class="content">
            <h3>{{ item.title }}</h3>
            <!-- <div class="ingredients-list">
              <div v-for="(ingredient, key) in item.usedIngredients" :key="key">
                {{ ingredient.name }}
              </div>
            </div> -->
            <button>View Ingredients</button>
          </div>
        </li>
      </ul>
      <div class="intro">
        <div class="intro_head">
          <h1>Trending Recipes</h1>
          <p>Cooked by peope with similar preferences</p>
        </div>
        <div class="pagination">
          <button>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
            >
              <path
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m14 7l-5 5l5 5"
              />
            </svg>
          </button>
          <button>
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 24 24"
            >
              <path
                fill="none"
                stroke="currentColor"
                stroke-linecap="round"
                stroke-linejoin="round"
                stroke-width="2"
                d="m10 17l5-5l-5-5"
              />
            </svg>
          </button>
        </div>
      </div>
      <ul>
        <li></li>
        <li></li>
        <li></li>
      </ul>
    </div>
  </main>
</template>

<style scoped>
.search_header {
  display: flex;
  align-items: center;
  justify-content: center;
  gap: 20px;
}
.search_header span {
  display: flex;
  align-items: center;
  justify-content: center;
  display: inline-flex;
  padding-inline-start: 1rem;
}
.search_header button {
  background-color: lightseagreen;
  color: #28455b;
  box-shadow: rgba(0, 0, 0, 0.08) 0px 4px 12px;
  border-radius: 10px;
  border: none;
  width: 50px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  color: white;
  &:hover {
    background-color: rgb(27, 156, 150);
  }
}
.input_wrp {
  background-color: white;
  box-shadow: rgba(0, 0, 0, 0.08) 0px 4px 12px;
  border-radius: 10px;
  display: flex;
  align-items: center;
  width: 500px;
  gap: 10px;
  border: 1px solid transparent;
  &:hover {
    border-color: lightseagreen;
  }
}
.input_wrp input {
  width: 90%;
  height: 48px;
  border: none;
  padding: 0 0.5rem;
  border-radius: 10px;
  font-size: 0.9rem;
  font-weight: 500;
  &::placeholder {
    font-weight: 550;
    color: rgb(149, 147, 167);
  }
}
.wrapper {
  width: 80%;
  padding: 2rem;
  max-width: 1400px;
  margin: 0 auto;
  display: flex;
  flex-direction: column;
  gap: 40px;
  max-height: 100vh;
  overflow-y: scroll;
}
.navigation {
  background-color: white;
  width: 18%;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  nav {
    display: flex;
    flex-direction: column;
    gap: 40px;
    width: 100%;
    padding-inline-start: 4rem;
  }
  a {
    color: #28455b;
    padding: 0rem 1rem;
    text-decoration: none;
    width: 100%;
    font-size: 1.1rem;
    border-inline-end: 2px solid gray;
  }
}

main {
  min-height: 100vh;
  display: flex;
  gap: 20px;
}
.intro {
  display: flex;
  align-items: center;
  justify-content: space-between;
  p {
    color: rgb(149, 147, 167);
    font-weight: 300;
  }
  .pagination {
    display: flex;
    align-items: center;
    justify-content: center;
    gap: 10px;
  }
  button {
    height: 35px;
    width: 55px;
    display: flex;
    align-items: center;
    justify-content: center;
    background-color: white;
    border: none;
    border-radius: 10px;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 4px 12px;
    cursor: pointer;
    &:hover {
      background-color: lightseagreen;
      color: white;
    }
  }
}
ul {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  list-style: none;
  gap: 20px;
  li {
    background-color: white;
    border-radius: 10px;
    position: relative;
    overflow: hidden;
    box-shadow: rgba(0, 0, 0, 0.08) 0px 4px 12px;
    .likes-icon {
      background-color: rgb(252, 50, 83);
      color: white;
      border-radius: 15px;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 5px;
      position: absolute;
      top: 1rem;
      right: 1rem;
      width: max-content;
      padding: 0 1rem;
      height: 35px;
      z-index: 100;
    }
    .content {
      padding: 1rem;
      display: flex;
      flex-direction: column;
      gap: 20px;
    }
    .img-bg {
      position: relative;
      height: 150px;
      &::before {
        content: "";
        position: absolute;
        inset: 0;
        background-color: rgba(0, 0, 0, 0.103);
        z-index: 2;
      }
    }
    .card_img {
      object-fit: cover;
      width: 100%;
      z-index: 1;
      height: 100%;
    }
    h3 {
      color: black;
    }
    .ingredients-list {
      display: flex;
      align-items: center;
      gap: 10px;
      div {
        background-color: #9caebd36;
        color: #28455b;
        border-radius: 10px;
        padding: 0.3rem 1rem;
        font-size: 0.8rem;
      }
    }
    button {
      width: 100%;
      height: 45px;
      background-color: rgb(248, 152, 43);
      color: white;
      border-radius: 20px;
      font-weight: 600;
      border: none;
      cursor: pointer;
      &:hover {
        background-color: rgb(223, 136, 38);
      }
    }
  }
}
</style>
