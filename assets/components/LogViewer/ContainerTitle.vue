<template>
  <div class="flex flex-1 items-center gap-2 truncate">
    <label class="swap swap-rotate">
      <input type="checkbox" v-model="pinned" />
      <carbon:star-filled class="swap-on text-secondary" />
      <carbon:star class="swap-off" />
    </label>
    <div class="inline-flex font-mono text-sm">
      <div v-if="config.hosts.length > 1" class="mobile-hidden font-thin">
        {{ container.hostLabel }}<span class="mx-2">/</span>
      </div>
      <div class="font-semibold">{{ container.name }}</div>
      <div
        class="mobile-hidden max-w-[1.5em] truncate transition-[max-width] hover:max-w-[400px]"
        v-if="container.isSwarm"
      >
        {{ container.swarmId }}
      </div>
    </div>
    <container-health :health="container.health" v-if="container.health"></container-health>
    <tag class="mobile-hidden font-mono" size="small">{{ container.image.replace(/@sha.*/, "") }}</tag>
  </div>
</template>

<script lang="ts" setup>
const { container } = useContainerContext();
const pinned = computed({
  get: () => pinnedContainers.value.has(container.value.name),
  set: (value) => {
    if (value) {
      pinnedContainers.value.add(container.value.name);
    } else {
      pinnedContainers.value.delete(container.value.name);
    }
  },
});
</script>
