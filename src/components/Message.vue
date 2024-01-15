<template>
    <div class="message">
        <p>{{ message }}</p>
    </div>
</template>

<script>
export default {
    props: {
        message: {
            type: String,
            required: true,
            // default: "No message",
        },
    },
};
</script>

<style lang="scss" scoped>
.message {
    text-align: center;
    padding: 20px;
    box-sizing: border-box;
}
p {
    color: red;
}
</style>
