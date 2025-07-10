<script setup>

</script>

<template>
  <div>
    <ul>
        <li><router-link to="/cube">Cube</router-link></li>
    </ul>
  </div>
</template>

<style scoped>
ul {
    width: 90%;
    margin: 0 auto;
    display: flex;
    flex-direction: column;
    gap: 8px;
}

li {
    background-color: rgba(0, 0, 0, .1);
    padding: 8px;
    border-radius: 9999px;

    &:hover {
        background-color: red;
    }
}

a {
    display: block;
}
</style>