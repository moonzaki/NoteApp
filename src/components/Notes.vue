<template>
    <div class="notes">
        <div
            class="note"
            :class="{ 'note--full': !grid }"
            v-for="(note, index) in notes"
            :key="index"
        >
            <div class="note-header" :class="{ 'note-header--full': !grid }">
                <h2
                    class="note-header__title"
                    :class="{ 'note-header__title--hidde': note.inChange }"
                    @click="changeNote(index)"
                >
                    {{ note.title }}
                </h2>
                <input
                    class="note-header__input"
                    :class="{ 'note-header__input--show': note.inChange }"
                    type="text"
                    :placeholder="note.title"
                    :value="note.title"
                    @keyup.esc="cancelChangeNote(index)"
                    @keyup.enter="submitChangeNote(index, $event)"
                    @input="handleInput"
                />
                <p class="note__close-btn" @click="removeNote(index)">x</p>
            </div>
            <div class="note__body">
                <p>{{ note.descr }}</p>
                <div class="note__body-info">
                    <span> {{ note.date }} </span>
                    <div
                        class="note__priority"
                        :class="setPriorityColor(priorities[note.priority])"
                    >
                        {{ note.priority }}
                    </div>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
export default {
    newTitle: "",
    props: {
        notes: {
            type: Array,
            required: true,
        },
        grid: {
            type: Boolean,
            required: true,
        },
        priorities: {
            type: Object,
            required: true,
        },
    },
    methods: {
        removeNote(idx) {
            console.log(`Note id - ${idx} removed`);
            this.$emit("remove", idx);
        },
        handleInput(event) {
            return event.target.value;
            // return event.target.value;
        },
        changeNote(idx) {
            this.$emit("changeNote", [idx, true]);
        },
        cancelChangeNote(idx) {
            this.$emit("cancelChangeNote", [idx, false]);
        },
        submitChangeNote(idx, event) {
            let newValue = this.handleInput(event);
            this.$emit("submitChangeNote", [idx, false, newValue]);
        },
        setPriorityColor(color) {
            return `note__priority--${color.toLowerCase()}`;
        },
    },
};
</script>

<style lang="scss">
.notes {
    display: flex;
    justify-content: space-between;
    align-items: center;
    flex-wrap: wrap;
    column-gap: 20px;
    row-gap: 20px;
    padding: 40px 0;
    box-sizing: border-box;
}

.note {
    width: calc((100% - 1 * 20px) / 2);

    padding: 18px 20px;
    background: white;
    box-sizing: border-box;
    transition: all 0.25s cubic-bezier(0.02, 0.01, 0.47, 1);
    box-shadow: 0 30px 30px rbga(0, 0, 0, 0.02);

    &--full {
        width: 100%;
    }

    &__priority {
        width: fit-content;
        padding: 8px 16px;
        box-sizing: border-box;
        border: 1px solid #999999;
        border-radius: 25px;
        margin: 15px 0 0;

        &--red {
            background: rgb(255, 0, 0, 0.3);
        }
        &--yellow {
            background: rgb(255, 255, 0, 0.3);
        }
        &--grey {
            background: rgb(128, 128, 128, 0.3);
        }
    }

    &:hover {
        box-shadow: 0 30px 30px rbga(0, 0, 0, 0.04);
        transform: translate(0, -6px);
        transition-delay: 0s !important;
    }

    &-head {
        display: flex;
        justify-content: space-between;
        align-items: center;
        margin-top: 40px;

        &__icons {
            display: flex;
            align-items: center;
            gap: 12px;

            svg {
                cursor: pointer;
                color: #999999;

                &.active {
                    color: #402caf;
                }
            }
        }

        h1 {
            font-size: 32px;
        }
    }

    &-header {
        display: flex;
        justify-content: space-between;
        align-items: center;

        &__title {
            &--hidde {
                display: none;
            }
        }

        &__input {
            align-self: center;
            margin: unset;
            width: fit-content;
            display: none;

            &--show {
                display: block;
            }
        }

        h2 {
            color: #402caf;
            font-size: 22px;
        }

        &--full {
            justify-content: center;
            gap: 16px;
        }
    }

    &__close-btn {
        display: flex;
        justify-content: center;
        width: 25px;
        height: 25px;
        align-items: center;
        border: 1px solid #402caf;
        border-radius: 2px;
        cursor: pointer;
    }

    &__body {
        p {
            margin: 20px 0;
        }

        span {
            font-size: 14px;
            color: #999;
        }

        &-info {
            display: flex;
            align-items: flex-end;
            justify-content: space-between;
        }
    }
}
</style>
