<script setup>
// import HelloWorld from './components/HelloWorld.vue'
import { ref, onMounted, computed, watch } from "vue";

const getListArray = ref([]);

const input_getListArray = () => {
  if (
    input_userContent.value.trim() === "" ||
    input_userContentCategory === null
  ) {
    return;
  }

  // console.log("Get listed");

  getListArray.value.push({
    content: input_userContent.value,
    category: input_userContentCategory.value,
    done: false,
    createdAt: new Date().getTime(),
  });
  input_userContent.value = ""
  input_userContentCategory.value = null
};

watch(
  getListArray,
  (setValue) => {
    localStorage.setItem("getListArray", JSON.stringify(setValue));
  },
  { deep: true }
);

const getListArrayFilter = computed(() =>
  getListArray.value.sort((a, b) => {
    return b.createdAt - a.createdAt;
  })
);

const input_userName = ref("");

watch(input_userName, (setValue) => {
  localStorage.setItem("input_userName", setValue);
});

const input_userContent = ref("");
const input_userContentCategory = ref(null);

const remove_ListArrayFilter = (getListArrayFilter) => {
  getListArray.value = getListArray.value.filter(
    (v) => v !== getListArrayFilter
  );
};

onMounted(() => {
  input_userName.value = localStorage.getItem("input_userName") || "";
  getListArray.value = JSON.parse(localStorage.getItem("getListArray")) || [];
});
</script>

<template>
  <main>
    <!-- Header -->
    <section>
      <header>
        <div>
          Hi
          <input
            type="text"
            placeholder="Enter Your Name here"
            v-model="input_userName"
          />
        </div>
      </header>
    </section>
    <!-- ToDo List -->
    <section>
      <div>Create a ToDo List</div>
      <form action="" @submit.prevent="input_getListArray">
        <div>What's up there</div>
        <input
          type="text"
          name="inputUserContent"
          placeholder="Enter Your Task here"
          v-model="input_userContent"
        />
        <!-- {{ input_userContent }} -->

        <div>Select a Category here</div>
        <div>
          <label for=""
            ><input
              type="radio"
              name="inputUserContentCategory"
              value="Personal"
              v-model="input_userContentCategory"
          /></label>
          <div>Personal</div>
          <label for=""
            ><input
              type="radio"
              name="inputUserContentCategory"
              value="Professional"
              v-model="input_userContentCategory"
          /></label>
          <div>Professional</div>
          <!-- {{ input_userContentCategory }} -->
          <input type="submit" value="Add in Bucket" />
        </div>
      </form>
    </section>

    <!-- {{ getListArrayFilter }} -->
    <!-- Bucket List -->
    <section>
      <div>Bucket List</div>
      <div>
        <div
          v-for="ListArrayFilter in getListArrayFilter"
          :class="`ListArrayFilter-Item ${ListArrayFilter.done && 'done'}`"
        >
          <label for="">
            <input type="checkbox" v-model="ListArrayFilter.done" />
            <!-- <div
              :class="`ListCategory ${
                ListArrayFilter.category == 'Personal' ? 'Personal' : 'Professional'
              }`"
            ></div> -->
            <div :class="`ListCategory ${ListArrayFilter.category}`"></div>
          </label>

          <div>
            <input type="text" v-model="ListArrayFilter.content" />
          </div>

          <div>
            <button @click="remove_ListArrayFilter(ListArrayFilter)">
              Delete
            </button>
          </div>
        </div>
      </div>
    </section>
  </main>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>
