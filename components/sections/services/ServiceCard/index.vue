<template>
  <router-link
    :to="serviceRouteAppendUrl"
    append
    class="service-card__container"
  >
    <div
      class="service-card__inner"
      @mouseenter="onCardMouseEnter"
      @mouseleave="onCardMouseLeave"
    >
      <img
        :src="serviceImage"
        :alt="service.name"
        class="service-card__image"
      >

      <div class="service-card__body">
        <h4 class="service-card__title">
          {{ service.name }}
        </h4>

        <div
          class="service-card__text"
        >
          {{ service.description }}
        </div>
      </div>
    </div>
  </router-link>
</template>

<script>
export default {
  name: 'ServiceCard',

  props: {
    service: {
      type: Object,
      default: () => ({}),
      required: true
    }
  },

  computed: {
    serviceImage () {
      return this.service.img ? `/images/aws/svg/SVG Light${this.service.img}` : '/images/aws/svg/SVG Light/_Group Icons/AWS-Cloud-alt_light-bg.svg'
    },

    serviceRouteAppendUrl () {
      return this.service.name.split(' ').join('_').toLowerCase()
    }
  },

  methods: {
    checkShouldExpandCard () {
      return window.matchMedia('(min-width: 908px)').matches
    },

    onCardMouseEnter (event) {
      if (!this.checkShouldExpandCard()) {
        return
      }

      const targetEl = event.target

      if (!targetEl) {
        return
      }

      targetEl.style.height = targetEl.scrollHeight?.toString() + 'px'
      targetEl.style.zIndex = '100'
    },

    onCardMouseLeave (event) {
      if (!this.checkShouldExpandCard()) {
        return
      }

      const TRANSITION_DURATION = 250
      const targetEl = event.target

      if (!targetEl) {
        return
      }

      targetEl.style.height = ''
      setTimeout(() => {
        targetEl.style.zIndex = ''
      }, TRANSITION_DURATION)
    }
  }
}
</script>

<style lang="scss">
.service-card {
  &__container {
    position: relative;
    height: 100%;
    display: block;
    text-decoration: none !important;
  }

  &__inner {
    width: 100%;
    height: 100%;
    min-height: 100%;
    color: var(--color-font);
    background-clip: border-box;
    border: 1px solid var(--color-border);
    border-radius: 0.25rem;
    transition: height 0.2s ease-in-out, box-shadow 0.3s ease-in-out;
    overflow: hidden;
    background-color: var(--color-bg-elevated-01);

    &:hover {
      color: var(--color-font);
    }
  }

  &__image {
    width: 100%;
  }

  &__body {
    padding: 0.45rem 1.25rem;
  }

  &__title {
    margin-bottom: 0.75rem;
  }

  @media (min-width: 908px) {
    &__container {
      height: 100%;
    }

    &__inner {
      position: absolute;
      top: 0;
      left: 0;

      &:hover {
        box-shadow: 1px 8px 25px -7px var(--color-box-shadow);
      }
    }
  }
}
</style>
