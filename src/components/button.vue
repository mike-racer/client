<template>
    <div>
        <button class="customButton">{{ isiaja }}</button>
    </div>
</template>
<script>
export default {
    name: 'button',
    props: ["isiaja"]
}
</script>
<style scoped>
.customButton{
    background: transparent;
    color: black;
}
.customButton:hover{
    background: transparent;
    color: red;
    border-color: red;
    border-radius: 5px;
}
</style>


