<template>
    <v-container fluid>
        <v-row class="my-2" cols="12" justify="center">
            <v-col cols="12">
                <v-card flat class="text-center">
                    <v-card-title>
                        <v-spacer />
                            <h2 class="text-uppercase primary--text">{{board.title}}</h2>
                        <v-spacer />
                    </v-card-title>
                </v-card>
            </v-col>
        </v-row>
        <v-row cols="12" :justify="justifyByWidth">
            <board-list 
                v-for="(list, listIndex) in board.lists" 
                :key="listIndex"
                :listIndex="listIndex"
                v-bind="$props" 
            />

            <v-col :cols="calculatedCols" class="text-center">
                <v-btn text dark block @click="addNewList">
                    <v-icon dense>mdi-plus</v-icon>
                    {{addListBtnTitle}}
                </v-btn>
            </v-col>
        </v-row>
    </v-container>
</template>

<script>
import BoardList from '../components/board-list.vue'

export default {
    props: ['boardId'],
    components: {
        BoardList
    },
    data() {
        return {
            addListBtnTitle: 'add another list'
        }
    },
    methods: {
        addNewList() {
            const data = {
                boardId: this.boardId,
                list: { title: 'New List', items: [] }
            }
            this.$store.dispatch('addNewListToBoard', data)
        }
    },
    computed: {
    board() {
      return this.$store.getters.getBoard(this.boardId)
    },
    calculatedCols() {
        switch(this.$vuetify.breakpoint.name) {
            case 'sm':
                return 6
            case 'xs':
                return 11
            default:
                return 3
        }
    },
    justifyByWidth() {
        return this.$vuetify.breakpoint.name === 'xs' ? 'center' : undefined 
    }
  }
}
</script>

<style>

</style>