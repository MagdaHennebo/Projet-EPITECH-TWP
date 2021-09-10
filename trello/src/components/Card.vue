<template>
    <div class="card">
        <h1 id="titlePost" class="card">test</h1>
        <slot/>
    </div>
</template>

<script>
export default {
  name: 'Card'
}
</script>

<style scoped>
.card{
  background: var(--color-white);
  padding:1.4rem;
  border-radius: 0.4rem;
  box-shadow: 0 1px 0 rgba(9, 30, 66, 0.25);
  font-size: 1.6rem;
  margin-bottom: 0.8rem;
  cursor: pointer;
}

</style>
