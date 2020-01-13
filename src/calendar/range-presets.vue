<template>
  <div :class="[`${prefixClass}-range-presets`]">
    <ul>
      <li
        v-for="(preset, index) in presets"
        :key="index"
        :class="{
          selected:
            preset.from.toDateString() == selected.from.toDateString() &&
            preset.to.toDateString() == selected.to.toDateString(),
        }"
        @click="handleClick(preset)"
      >
        {{ preset.label }}
      </li>
    </ul>
  </div>
</template>

<script>
import { subDays, subMonths, startOfMonth, endOfMonth } from 'date-fns';
import { getLocaleFieldValue } from '../locale';

export default {
  props: {
    value: {},
  },
  inject: {
    t: {
      default: () => getLocaleFieldValue,
    },
    prefixClass: {
      default: 'mx',
    },
  },
  data() {
    return {
      selected: {
        from: null,
        to: null,
      },
      presets: [
        {
          label: 'Today',
          from: new Date(),
          to: new Date(),
        },
        {
          label: 'Yesterday',
          from: subDays(new Date(), 1),
          to: subDays(new Date(), 1),
        },
        {
          label: 'Last 7 Days',
          from: subDays(new Date(), 6),
          to: new Date(),
        },
        {
          label: 'Last 30 Days',
          from: subDays(new Date(), 29),
          to: new Date(),
        },
        {
          label: 'This Month',
          from: startOfMonth(new Date()),
          to: endOfMonth(new Date()),
        },
        {
          label: 'Last Month',
          from: startOfMonth(subMonths(new Date(), 1)),
          to: endOfMonth(subMonths(new Date(), 1)),
        },
      ],
    };
  },
  watch: {
    value: {
      immediate: true,
      handler(value) {
        this.selected = {
          from: value[0],
          to: value[1],
        };
      },
    },
  },
  methods: {
    handleClick(preset) {
      this.selected = preset;
      this.$emit('selected', preset);
    },
  },
};
</script>
