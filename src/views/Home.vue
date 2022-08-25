<template>
  <div>
    <Counter
      purpose="2023"
      :year="2023"
      :month="0"
      :date="1"
      :hour="0"
      :minute="0"
      :second="0"
      :millisecond="0"
    />
  </div>
</template>

<script>
import Counter from "@/components/Counter.vue";

export default {
  components: {
    Counter,
  },
};
</script>
