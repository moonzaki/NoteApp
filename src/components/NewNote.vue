<template>
    <div class="new-note">
        <label>Title</label>
        <input v-model="note.title" type="text" />
        <label>Description</label>
        <textarea v-model="note.descr" style="resize: none"></textarea>
        <label>Choice priority</label>
        <select v-model="note.priority" required>
            <option v-for="(value, name, index) in priorities" :key="index">
                {{ name }}
            </option>
        </select>
        <button class="btn btnPrimary" @click="addNote">New note</button>
    </div>
</template>

<script>
export default {
    props: {
        note: {
            type: Object,
            required: true,
        },
        priorities: {
            type: Object,
            required: true,
        },
    },
    methods: {
        addNote() {
            console.log(this.note);
            this.$emit("addNote", this.note);
        },
    },
};
</script>

<style lang="scss">
.new-note {
    display: flex;
    flex-direction: column;
    gap: 24px;
    text-align: center;
    label {
        margin-bottom: unset;
    }
    input {
        margin: unset;
    }

    textarea {
        margin: unset;
    }

    select {
        width: fit-content;
        margin: 0 auto;
    }

    button {
        width: fit-content;
        margin: 0 auto;
    }
}
</style>
