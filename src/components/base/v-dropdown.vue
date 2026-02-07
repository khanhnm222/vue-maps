<template>
  <div class="dropdown">
    <div class="dropdown-select" @click="openDropDownOptions">
      <span>{{ selectedItem.name }}</span>
      <keyboard-arrow-down-icon :class="['icon', { rotate: showDropDownOptions }]" size="18" />
    </div>
    <div
      v-if="showDropDownOptions"
      class="dropdown-options"
      v-click-away="
        () => {
          showDropDownOptions = false
        }
      "
    >
      <div
        v-for="(item, index) in getOptions"
        :key="`${index}_${item.id}`"
        class="item"
        @click="selectItem(item.id)"
      >
        <span>{{ item.name }}</span>
      </div>
    </div>
  </div>
</template>

<script lang="ts">
import KeyboardArrowDownIcon from '../icons/KeyboardArrowDownIcon.vue'
import { computed, ref, type PropType } from 'vue'

interface OptionItem {
  id: string
  name: string
  value: string
}

type OptionMap = Record<string, OptionItem>

export default {
  props: {
    value: {
      type: String,
    },
    options: {
      type: Object as PropType<OptionMap>,
      required: true,
    },
  },
  components: {
    KeyboardArrowDownIcon,
  },
  setup(props, { emit }) {
    const showDropDownOptions = ref(false)

    const selectedItem = computed(() => {
      return props.options[props.value || ''] || { id: '', name: 'Select an option', value: '' }
    })

    const getOptions = computed(() => {
      return Object.values(props.options)
    })

    const openDropDownOptions = () => {
      if (!showDropDownOptions.value) {
        showDropDownOptions.value = true
      }
    }

    const selectItem = (itemId: any) => {
      emit('select', itemId)
      showDropDownOptions.value = false
    }

    return { selectedItem, getOptions, showDropDownOptions, openDropDownOptions, selectItem }
  },
}
</script>

<style lang="scss" scoped>
.dropdown {
  display: flex;
  flex-direction: column;
  flex: 1;
  position: relative;

  .dropdown-select {
    display: flex;
    flex-direction: row;
    align-items: center;
    padding: 5px;
    border-radius: 3px;
    height: 24px;

    .icon {
      transition: 0.1s all ease-in-out;
      &.rotate {
        transform: rotate(180deg);
      }
    }

    span {
      margin-left: 10px;
      flex: 1;
    }

    &:hover {
      cursor: pointer;
      background: var(--color-secondary-light);
    }
  }

  .dropdown-options {
    display: flex;
    flex-direction: column;
    max-height: 300px;
    overflow: auto;
    border: 1px solid var(--border-color);
    border-radius: 3px;
    box-shadow: 0px 10px 20px rgba(0, 0, 0, 0.1);
    background: var(--color-secondary);
    position: absolute;
    top: 30px;
    left: 0;
    width: 100%;
    z-index: 9;

    .item {
      display: flex;
      flex-direction: row;
      align-items: center;
      padding: 5px;
      border-radius: 3px;

      span {
        margin-left: 10px;
        flex: 1;
      }

      &:hover {
        cursor: pointer;
        background: var(--color-secondary-light);
      }
    }
  }
}
</style>
