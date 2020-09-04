<template>
  <div class="link-shadow" id="link">
    <div class="link-paper">
      <div class="paper-controls">
        <input class="link-box" type="text" placeholder="Enter a URL..." />
        <div class="controls-action">
          <div class="action-link">Link</div>
          <div class="action-relink">Unlink</div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "link",
  data() {
    return {};
  },
  methods: {
  },
};
</script>

<style scoped lang="stylus">
@import './linkGenerator.styl';
</style>
