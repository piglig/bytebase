<template>
  <component
    :is="link ? 'router-link' : tag"
    v-bind="bindings"
    class="inline-flex items-center gap-x-1"
    :class="[link && !plain && 'normal-link', link && 'hover:underline']"
  >
    <span class="line-clamp-1" :class="textClass">
      {{ environmentV1Name(environment) }}
      {{ suffix }}
    </span>
    <ProductionEnvironmentV1Icon
      v-if="showIcon"
      :environment="environment"
      :class="iconClass ?? '!text-current'"
      :tooltip="tooltip"
    />
  </component>
</template>

<script lang="ts" setup>
import { computed } from "vue";
import { Environment } from "@/types/proto/v1/environment_service";
import { VueClass, environmentV1Name, environmentV1Slug } from "@/utils";
import ProductionEnvironmentV1Icon from "./ProductionEnvironmentV1Icon.vue";

const props = withDefaults(
  defineProps<{
    environment: Environment;
    tag?: string;
    link?: boolean;
    plain?: boolean;
    iconClass?: VueClass;
    tooltip?: boolean;
    suffix?: string;
    showIcon?: boolean;
    textClass?: string;
  }>(),
  {
    tag: "span",
    link: true,
    plain: false,
    iconClass: undefined,
    tooltip: false,
    suffix: "",
    showIcon: true,
    textClass: "",
  }
);

const bindings = computed(() => {
  if (props.link) {
    return {
      to: `/environment/${environmentV1Slug(props.environment)}`,
      activeClass: "",
      exactActiveClass: "",
      onClick: (e: MouseEvent) => {
        e.stopPropagation();
      },
    };
  }
  return {};
});
</script>
