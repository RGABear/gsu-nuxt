<template>
  <div class="card">
    <CardTipList :tips="course.specs" :time="course.duration" />

    <img
      class="card__img"
      v-if="course.banner_image"
      :src="course.banner_image"
      alt="course image"
    />

    <h3 class="card__title text text_h3 text_dark">
      {{ course.title }}
    </h3>

    <div class="card__buttons">
      <NuxtLink class="nuxt-link" to="/basket">
        <button
          class="button card__button button_dark"
          @click="addToBasket(course)"
        >
          Записаться
        </button>
      </NuxtLink>

      <NuxtLink class="nuxt-link" :to="`/courses/${course.id}`">
        <button class="button card__button button_black-bordered">
          Подробнее
        </button>
      </NuxtLink>
    </div>
  </div>
</template>

<script>
export default {
  name: "ProgramCard",
  props: {
    course: {
      type: Object,
      require: true,
    },
  },
  setup() {
    const { addToBasket } = useUtils();
    return {
      addToBasket,
    };
  },
};
</script>

<style lang="less" scoped>
@import "assets/core.less";

.nuxt-link {
  width: 100%;
}

.card__button {
  width: 100%;
}

.card {
  box-sizing: border-box;
  padding: 12px;
  height: auto;

  overflow: hidden;

  display: flex;
  flex-direction: column;
  justify-content: space-between;
  row-gap: 12px;

  width: 100%;
  border-radius: 8px;
  background: @LightGreyColor;

  @media @min760 {
    padding: 20px;
  }
}

.card__buttons {
  display: flex;
  gap: 12px;
  flex-direction: column;

  @media @min1200 {
    flex-direction: row;
  }
}

.card__img {
  width: 100%;
  height: 230px;
  border-radius: 8px;

  object-fit: cover;
}

.card__tip-list {
  display: flex;
  gap: 8px;
  flex-wrap: wrap;
}
</style>
