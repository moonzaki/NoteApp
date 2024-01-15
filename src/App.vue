<template>
    <section>
        <!-- app -->
        <div class="wrapper">
            <div class="wrapper__container">
                <section>
                    <div class="container">
                        <!-- message -->
                        <message v-if="message" :message="message" />
                        <!-- new note -->
                        <newNote
                            :note="note"
                            :priorities="priorities"
                            @addNote="addNote"
                        />
                        <div class="note-head">
                            <!-- title -->
                            <h1>{{ title }}</h1>

                            <!-- search -->
                            <search
                                :value="search"
                                placeholder="Find your note"
                                @search="search = $event"
                            />
                            <div class="note-head__icons">
                                <svg
                                    :class="{ active: grid }"
                                    @click="grid = true"
                                    xmlns="http://www.w3.org/2000/svg"
                                    width="24"
                                    height="24"
                                    viewBox="0 0 24 24"
                                    fill="none"
                                    stroke="currentColor"
                                    stroke-width="2"
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                >
                                    <rect
                                        x="3"
                                        y="3"
                                        width="7"
                                        height="7"
                                    ></rect>
                                    <rect
                                        x="14"
                                        y="3"
                                        width="7"
                                        height="7"
                                    ></rect>
                                    <rect
                                        x="14"
                                        y="14"
                                        width="7"
                                        height="7"
                                    ></rect>
                                    <rect
                                        x="3"
                                        y="14"
                                        width="7"
                                        height="7"
                                    ></rect>
                                </svg>
                                <svg
                                    :class="{ active: !grid }"
                                    @click="grid = false"
                                    xmlns="http://www.w3.org/2000/svg"
                                    width="24"
                                    height="24"
                                    viewBox="0 0 24 24"
                                    fill="none"
                                    stroke="currentColor"
                                    stroke-width="2"
                                    stroke-linecap="round"
                                    stroke-linejoin="round"
                                >
                                    <line x1="8" y1="6" x2="21" y2="6"></line>
                                    <line x1="8" y1="12" x2="21" y2="12"></line>
                                    <line x1="8" y1="18" x2="21" y2="18"></line>
                                    <line x1="3" y1="6" x2="3" y2="6"></line>
                                    <line x1="3" y1="12" x2="3" y2="12"></line>
                                    <line x1="3" y1="18" x2="3" y2="18"></line>
                                </svg>
                            </div>
                        </div>

                        <!-- note list -->
                        <notes
                            :notes="notesFilter"
                            :grid="grid"
                            :priorities="priorities"
                            @remove="removeNote"
                            @changeNote="changeNote"
                            @cancelChangeNote="cancelChangeNote"
                            @submitChangeNote="submitChangeNote"
                        />
                    </div>
                </section>
            </div>
        </div>
    </section>
</template>

<script>
import message from "@/components/Message.vue";
import newNote from "@/components/NewNote.vue";
import notes from "@/components/Notes.vue";
import search from "@/components/Search.vue";

export default {
    components: {
        message,
        newNote,
        notes,
        search,
    },
    data() {
        return {
            title: "Notes App",
            search: "",
            message: null,
            grid: true,
            // priorities: ["Important", "Average", "Short"],
            note: {
                title: "",
                descr: "",
                priority: "",
                inChange: false,
            },
            priorities: {
                Important: "Red",
                Average: "Yellow",
                Short: "Grey",
            },
            notes: [
                {
                    title: "First Note",
                    descr: "Description for first note",
                    date: new Date(Date.now()).toLocaleString(),
                    priority: "Important",
                    inChange: false,
                },
                {
                    title: "Second Note",
                    descr: "Description for second note",
                    date: new Date(Date.now()).toLocaleString(),
                    priority: "Average",
                    inChange: false,
                },
                {
                    title: "Third Note",
                    descr: "Description for third note",
                    date: new Date(Date.now()).toLocaleString(),
                    priority: "Short",
                    inChange: false,
                },
            ],
        };
    },
    computed: {
        notesFilter() {
            let array = this.notes;
            let search = this.search;

            if (!search) return array;
            search = search.trim().toLowerCase();
            array = array.filter((item) => {
                if (item && item.title.toLowerCase().indexOf(search) !== -1) {
                    return item;
                }
            });

            return array;
        },
    },
    methods: {
        addNote() {
            let { title, descr, priority } = this.note;
            if (title === "") {
                this.message = "Title node can`t be blank!";
                return false;
            }

            this.notes.push({
                title,
                descr,
                date: new Date(Date.now()).toLocaleString(),
                priority,
                inChange: false,
            });

            this.note.title = "";
            this.note.descr = "";
            this.note.priority = "";
            this.message = null;
        },
        removeNote(idx) {
            this.notes.splice(idx, 1);
        },
        changeNote(props) {
            this.notes[props[0]].inChange = props[1];
        },
        cancelChangeNote(props) {
            this.notes[props[0]].inChange = props[1];
        },
        submitChangeNote(props) {
            this.notes[props[0]].inChange = props[1];
            this.notes[props[0]].title = props[2];
        },
    },
};
</script>

<style></style>
