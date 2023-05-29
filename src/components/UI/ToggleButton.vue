<template>
    <button :class="{pressed: isPressed}"
            class="btn"
            @click="isPressed=!isPressed"
    >
        <slot></slot>
    </button>
</template>

<script>
export default {
    name: 'toggle-button',
    props: {
        isPressed: false
    }
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
}

.btn:hover {
    border: 3px solid rgb(81, 138, 88);
}


.pressed {
    background: #eeffcc;
}
</style>