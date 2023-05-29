<template>
    <button class="btn">
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: 'my-button'
}
</script>

<style scoped>
.btn {
    padding: 10px 15px;
    background: white;
    color: rgb(81, 138, 88);
    border: 3px solid white;
    border-radius: 4px;
    cursor: pointer;
    font-weight: bold;

    //box-shadow: rgb(235, 235, 235) 0px 1px 0px;

}
.btn:hover {
    border: 3px solid rgb(81, 138, 88);
}
</style>