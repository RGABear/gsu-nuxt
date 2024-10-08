<template>
  <div class="search-block wrapper wrapper_paddings">
    <h2 class="text text_h2 text_dark">Обучение</h2>

    <ProgramSearchBar @search="search()" v-model:value="searchValue" />

    <SearchTipList :tips="categories" v-model:selected="category" />

    <div class="search-block__filters">
      <DropDown
        :title="'Специализация'"
        :options="specs"
        v-model:selected="selectedSpec"
        class="search-block__drop-down"
      />

      <DropDown
        :title="'Слушатели'"
        :options="studentCategories"
        v-model:selected="selectedStudentCategories"
        class="search-block__drop-down"
      />

      <DropDown
        :title="'Количество часов'"
        :options="durations"
        v-model:selected="selectedDuration"
        class="search-block__drop-down"
      />
    </div>

    <CourseCardList v-model:courses="courses" class="search-block__card-list" />

    <div class="pagination">
      <PaginationBar :size="amount" :count="count" v-model:page="currentPage" />
    </div>
  </div>
</template>

<script setup>
import { API } from "~/constants/index.js";
import { toValue, watch } from "vue";
import { useRoute, useRouter } from "vue-router";

const selectedSpec = ref({});
const selectedDuration = ref({});
const selectedStudentCategories = ref({});
const category = ref(NaN);
const courses = ref([]);
const currentPage = ref(1);
const amount = 5;
const searchValue = ref("");

/* search request */
async function search(noUpdated = undefined) {
  if (!noUpdated) currentPage.value = 1;

  const req = {
    start: toValue(currentPage) * amount - amount,
    amount: amount,
    sort: 0,
    category: toValue(category),
    search_value: toValue(searchValue),

    search_student_category: toValue(selectedStudentCategories).id,
    search_spec: toValue(selectedSpec).id,
    search_duration: toValue(selectedDuration).id,
  };

  const { data: page } = await useFetch(API + "/page/learning", {
    method: "POST",
    body: req,
  });

  courses.value = toValue(page).page.courses;
  durations.value = toValue(page).page.durations;
  specs.value = toValue(page).page.specs;
  studentCategories.value = toValue(page).page.student_categories;
  count.value = toValue(page).page.total_courses_amount;
}

/* search request to get options info */
const route = useRoute();
let firstCategory = route.query.id ? route.query.id : 0;

const firstRequestBody = {
  start: 0,
  amount: amount,
  sort: 0,
  category: firstCategory,
};

const { data: page } = await useFetch(API + "/page/learning", {
  method: "POST",
  body: firstRequestBody,
});

const durations = ref(toValue(page).page.durations);
const specs = ref(toValue(page).page.specs);
const studentCategories = ref(toValue(page).page.student_categories);
const categories = toValue(page).page.categories;
courses.value = toValue(page).page.courses;
const count = ref(toValue(page).page.total_courses_amount);

/* Watchers */
watch(
  () => route.query.id,
  (newId) => {
    category.value = newId || 0;

    window.scrollTo({
      top: 0,
      behavior: "smooth",
    });
  }
);

watch(currentPage, async (newVal) => {
  currentPage.value = newVal;
  await search(true);
});

watch(category, async (newVal) => {
  category.value = newVal;
  await search(false);
});

/* Filters */
watch(selectedSpec, async (newVal) => {
  await search(false);
});
watch(selectedDuration, async (newVal) => {
  await search(false);
});
watch(selectedStudentCategories, async (newVal) => {
  await search(false);
});
</script>

<style lang="less" scoped>
@import "assets/core.less";

.search-block {
  padding-top: 40px;

  @media @min1200 {
    padding-top: 80px;
  }
}

.search-block__drop-down {
  background: @LightGreyColor;
}

.search-block__card-list {
  margin-top: 32px;

  @media @min760 {
    margin-top: 40px;
  }
}

.search-block__filters {
  width: 100%;
  display: flex;
  justify-content: space-between;
  flex-direction: column;
  gap: 12px;

  margin-top: 24px;

  @media @min580 {
    flex-direction: row;
  }
}
</style>
