<template>
    <div class="wrapper">
        <div class="utils">
            <button @click="clear">clear</button>
            <button @click="solve">solve</button>
            <select name="difficulties" id="difficulty" v-model="selected" @change="changeDifficulty">
                <option
                        v-for="(c, index) in this.challenges"
                        :value="c.difficulty"
                        :key="index"
                >
                    {{c.difficulty}}
                </option>
            </select>
        </div>
        <div class="field">
            <div v-for="(row, rowindex) in board" :key="rowindex" class="row">
                <div v-for="(cell, colindex) in row" :key="colindex" class="inline cell" contenteditable="true"
                     @blur="change($event, rowindex, colindex)">
                    {{cell === 0 ? '' : cell}}
                </div>
            </div>
        </div>
    </div>
</template>

<script>
    import Vue from 'vue'

    export default {
        name: "Board",
        data() {
            return {
                selected: "easy",
                board: [
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0]
                ],
                challenges: [
                    {
                        difficulty: 'easy',
                        board: [
                            [6, 0, 0, 2, 9, 0, 5, 0, 0],
                            [0, 8, 0, 0, 0, 0, 0, 0, 0],
                            [0, 5, 0, 4, 0, 0, 0, 3, 2],
                            [1, 2, 0, 3, 0, 0, 0, 8, 0],
                            [0, 0, 7, 0, 0, 0, 4, 0, 0],
                            [0, 4, 0, 0, 0, 5, 0, 2, 6],
                            [9, 1, 0, 0, 0, 8, 0, 6, 0],
                            [0, 0, 0, 0, 0, 0, 0, 7, 0],
                            [0, 0, 2, 0, 6, 4, 0, 0, 9],
                        ],
                    },
                    {
                        difficulty: 'medium',
                        board:
                            [
                                [3, 0, 6, 5, 0, 8, 4, 0, 0],
                                [5, 2, 0, 0, 0, 0, 0, 0, 0],
                                [0, 8, 7, 0, 0, 0, 0, 3, 1],
                                [0, 0, 3, 0, 1, 0, 0, 8, 0],
                                [9, 0, 0, 8, 6, 3, 0, 0, 5],
                                [0, 5, 0, 0, 9, 0, 6, 0, 0],
                                [1, 3, 0, 0, 0, 0, 2, 5, 0],
                                [0, 0, 0, 0, 0, 0, 0, 7, 4],
                                [0, 0, 5, 2, 0, 6, 3, 0, 0]
                            ]
                    }
                ],
            }
        },
        mounted() {
            this.board = this.challenges.find(e => e.difficulty === this.selected).board
        },
        methods: {

            changeDifficulty() {
                console.log(this.difficulty)
                console.log(this.challenges)
                console.log(this.board)
                this.board = this.challenges.find(e => e.difficulty === this.selected).board
            },

            change(e, row, col) {
                const val = parseInt(e.target.innerText, 10)
                if (!isNaN(val)) {
                    this.board[row][col] = val;
                } else {
                    e.target.innerText = ''
                }
            },
            clear() {
                this.board = [
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                    [0, 0, 0, 0, 0, 0, 0, 0, 0],
                ]
            },
            solve() {

                let [row, col] = this.findCandidatePosition()

                for (let candidateNumber = 1; candidateNumber <= 9; candidateNumber++) {
                    Vue.set(this.board[row], col, candidateNumber)
                    if (this.validate(null, row, col)) {
                        if (this.solve()) {
                            return true;
                        }
                        Vue.set(this.board[row], col, 0)
                    }
                    Vue.set(this.board[row], col, 0)
                }
                return false;
            },
            findCandidatePosition() {
                for (let r = 0; r < 9; r++) {
                    for (let c = 0; c < 9; c++) {
                        if (this.board[r][c] === 0) {
                            return [r, c]
                        }
                    }
                }
            },
            validate(e, row, col) {
                let candidate = this.board[row][col];
                for (let i = 0; i < 9; i++) {
                    if (i !== col && this.board[row][i] === candidate) {
                        console.log(`Candidate ${candidate} already exists in row ${row}-${i}`)
                        return false;
                    }
                }
                for (let i = 0; i < 9; i++) {
                    if (i !== row && this.board[i][col] === candidate) {
                        console.log(`Candidate ${candidate} already exists in col ${i}-${col}`)
                        return false;
                    }
                }
                let qrow = (row / 3) | 0;
                let qcol = (col / 3) | 0;
                for (let i = 3 * qrow; i < qrow + 3; i++) {
                    for (let j = 3 * qcol; j < qcol + 3; j++) {
                        if (i !== row && j !== col && this.board[i][j] === candidate) {
                            console.log(`Candidate ${candidate} already exists in quadrant ${qrow}-${qcol}`)
                            return false;
                        }
                    }
                }
                return true;
            }
        }
    }
</script>

<style scoped>
    .inline {
        display: inline-block;
    }

    .cell {
        width: 30px;
        height: 30px;
        border: 1px solid black;
        text-align: center;
        vertical-align: middle;
        line-height: 30px;
    }

    .field {
        display: inline-block;
        border: 3px solid black;
    }

    .cell:nth-of-type(3n) {
        border-right: 3px solid black;
    }

    .row:nth-of-type(3n) {
        border-bottom: 2px solid black;
    }

</style>