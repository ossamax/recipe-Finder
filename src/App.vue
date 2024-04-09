<script>
import axios from "axios";
import { onMounted, ref, watch, watchEffect } from "vue";

export default {
  setup() {
    const data = ref([]);
    const inputValue = ref("");
    const loading = ref(false);
    const error = ref(null);

    const fetchData = () => {
      loading.value = true;

      axios
        .get("https://api.edamam.com/api/recipes/v2", {
          params: {
            type: "public",
            q: inputValue.value.trim() ? inputValue.value.trim() : "all",
            app_id: "56693293",
            app_key: "b8f59935f387ea92845674ddcf84cafc",
          },
        })
        .then((response) => {
          data.value = response.data.hits;
        })
        .catch((error) => {
          error.value = error;
          console.error("Error fetching recipes:", error);
        })
        .finally(() => {
          loading.value = false;
        });
    };

    // Fetch data when component is mounted

    const debounceFunction = debounce(fetchData, 600);
    // onMounted(() => {
    //   if (inputValue.value !== "") {
    //     debounceFunction();
    //   }
    // });
    watch(inputValue, () => {
      if (inputValue.value !== "") {
        debounceFunction();
      }
    });

    return {
      data,
      inputValue,
      loading,
      error,
    };
  },
};

function debounce(func, delay) {
  let timeoutId;
  return function (...args) {
    clearTimeout(timeoutId);
    timeoutId = setTimeout(() => {
      func.apply(this, args);
    }, delay);
  };
}
</script>

<template>
  <main>
    <div class="wrapper">
      <header class="search_header">
        <h1>Discover Recipes</h1>
        <p>
          Oh, no! You seem to have wandered into a food desert! You must be
          hungry...
        </p>
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
      </header>
      <ul v-if="data.length > 0">
        <li v-for="(item, index) in data" :key="index">
          <div class="likes-icon">
            <svg
              xmlns="http://www.w3.org/2000/svg"
              width="20"
              height="20"
              viewBox="0 0 256 256"
            >
              <path
                fill="currentColor"
                d="M178 36c-20.09 0-37.92 7.93-50 21.56C115.92 43.93 98.09 36 78 36a66.08 66.08 0 0 0-66 66c0 72.34 105.81 130.14 110.31 132.57a12 12 0 0 0 11.38 0C138.19 232.14 244 174.34 244 102a66.08 66.08 0 0 0-66-66m-5.49 142.36a328.69 328.69 0 0 1-44.51 31.8a328.69 328.69 0 0 1-44.51-31.8C61.82 159.77 36 131.42 36 102a42 42 0 0 1 42-42c17.8 0 32.7 9.4 38.89 24.54a12 12 0 0 0 22.22 0C145.3 69.4 160.2 60 178 60a42 42 0 0 1 42 42c0 29.42-25.82 57.77-47.49 76.36"
              />
            </svg>
          </div>
          <div class="img-bg">
            <img :src="item.recipe.image" alt="image" class="card_img" />
          </div>
          <div class="content">
            <h3>{{ item.recipe.label }}</h3>
            <div class="details">
              <div class="time">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="18"
                  height="18"
                  viewBox="0 0 24 24"
                >
                  <path
                    fill="currentColor"
                    d="M11.99 2C6.47 2 2 6.48 2 12s4.47 10 9.99 10C17.52 22 22 17.52 22 12S17.52 2 11.99 2M12 20c-4.42 0-8-3.58-8-8s3.58-8 8-8s8 3.58 8 8s-3.58 8-8 8m.5-13H11v6l5.25 3.15l.75-1.23l-4.5-2.67z"
                  /></svg
                > 30 min
              </div>
              <div class="time">
                <svg
                  xmlns="http://www.w3.org/2000/svg"
                  width="18"
                  height="18"
                  viewBox="0 0 24 24"
                >
                  <g fill="none">
                    <circle
                      cx="12"
                      cy="12"
                      r="10"
                      stroke="currentColor"
                      stroke-width="1.5"
                    />
                    <path
                      stroke="currentColor"
                      stroke-linecap="round"
                      stroke-width="1.5"
                      d="M9 16c.85.63 1.885 1 3 1s2.15-.37 3-1"
                    />
                    <path
                      fill="currentColor"
                      d="M16 10.5c0 .828-.448 1.5-1 1.5s-1-.672-1-1.5s.448-1.5 1-1.5s1 .672 1 1.5"
                    />
                    <ellipse
                      cx="9"
                      cy="10.5"
                      fill="currentColor"
                      rx="1"
                      ry="1.5"
                    />
                  </g></svg
                >2 persons
              </div>
            </div>
            <div class="reviews">
              <div class="stars">
                <div v-for="star in [1, 2, 3, 4, 5]" :key="star" class="star">
                  <svg
                    xmlns="http://www.w3.org/2000/svg"
                    width="20"
                    height="20"
                    viewBox="0 0 24 24"
                  >
                    <path
                      fill="orange"
                      d="m12 16.3l-3.7 2.825q-.275.225-.6.213t-.575-.188t-.387-.475t-.013-.65L8.15 13.4l-3.625-2.575q-.3-.2-.375-.525t.025-.6t.35-.488t.6-.212H9.6l1.45-4.8q.125-.35.388-.538T12 3.475t.563.188t.387.537L14.4 9h4.475q.35 0 .6.213t.35.487t.025.6t-.375.525L15.85 13.4l1.425 4.625q.125.35-.012.65t-.388.475t-.575.188t-.6-.213z"
                    />
                  </svg>
                </div>
              </div>
              (20)
            </div>
            <!-- <button>View Ingredients</button> -->
          </div>
        </li>
      </ul>
      <div v-if="data.length == 0" class="empty_img">
        <img src="/empty_search.svg" class="img" alt="empty" />
      </div>
    </div>
  </main>
</template>

<style scoped>
.search_header {
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
}
p {
  color: rgb(149, 147, 167);
  font-weight: 300;
  text-align: center;
}
.empty_img {
  display: flex;
  align-items: center;
  justify-content: center;
  img {
    width: 100%;
    max-width: 400px;
    height: 300px;
  }
}
.search_header span {
  display: flex;
  align-items: center;
  justify-content: center;
  display: inline-flex;
  padding-inline-start: 1rem;
}
.input_wrp {
  background-color: white;
  box-shadow: rgba(0, 0, 0, 0.04) 0px 3px 5px;
  border-radius: 25px;
  display: flex;
  align-items: center;
  max-width: 500px;
  gap: 10px;
  border: 1px solid transparent;
  margin-top: 1rem;
  @media screen and (max-width: 600px) {
    width: 100%;
  }
  &:hover {
    border-color: lightseagreen;
  }
}
.input_wrp input {
  width: 90%;
  height: 48px;
  border: none;
  padding: 0 0.5rem;
  border-radius: 25px;
  font-size: 0.9rem;
  font-weight: 500;
  &::placeholder {
    font-weight: 500;
    color: rgb(132, 130, 151);
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
  @media screen and (max-width: 600px) {
    width: 95%;
    padding: 2rem 1rem;
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
  gap: 25px;
  @media screen and (max-width: 600px) {
    grid-template-columns: repeat(1, 1fr);
  }
  li {
    border-radius: 8px;
    position: relative;
    overflow: hidden;
    cursor: pointer;
    box-shadow: rgba(0, 0, 0, 0.04) 0px 3px 5px;
    background-color: white;
    &:hover {
      transform: translateY(-5px);
    }
    .likes-icon {
      background-color: #28455b;
      color: white;
      border-radius: 100%;
      display: flex;
      align-items: center;
      justify-content: center;
      gap: 5px;
      position: absolute;
      top: 1rem;
      right: 1rem;
      width: 40px;
      height: 40px;
      z-index: 100;
    }
    .reviews {
      display: flex;
      align-items: center;
      gap: 5px;
      font-weight: 400;
      font-size: 0.8rem;
    }
    .stars {
      display: flex;
      align-items: center;
    }
    .star {
      display: flex;
      align-items: center;
      justify-content: center;
    }
    .content {
      padding: 1rem;
      display: flex;
      flex-direction: column;
      gap: 10px;
    }
    .details {
      display: flex;
      align-items: center;
      gap: 20px;
      div {
        display: flex;
        align-items: center;
        justify-content: center;
        font-weight: 400;
        font-size: 0.8rem;
        gap: 5px;
      }
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
      border-radius: 5px;
    }
    h3 {
      color: black;
      font-weight: 500;
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
      height: 40px;
      background-color: transparent;
      color: lightseagreen;
      border-radius: 20px;
      font-weight: 600;
      border: none;
      cursor: pointer;
      border: 1px solid lightseagreen;
      &:hover {
        background-color: lightseagreen;
        color: white;
      }
    }
  }
}
</style>
