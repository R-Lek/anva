<template>
    <tbody>
        <tr class="row" v-for="i in Math.ceil(puzzlePieces.length / 4)" :key="i">
            <td v-for="piece in puzzlePieces.slice((i-1)*4, i*4)" :key="piece">
                <img :id="counter()" @click="emit" class="puzzle-part" :src="`/assets/${piece}.png`">
            </td>
        </tr>
    </tbody>
</template>

<script>
 export default {
    name: 'PuzzlePiece',
        data() {
            return {
                count: 0
            };
        },
        props: {
            puzzlePieces: Array
        },
        methods: {
            emit(e) {
                this.$emit('piece-id', (e.currentTarget.getAttribute('id')));
            },
            counter() {
                // This causes a bug because of the reevalutation of this.count, unsure what the best workaround is
                if (this.count < 16) {
                    return this.count += 1;
                } else {
                    return this.count = 1;
                }
            }
        }
    }
</script>

<style scoped>
    .puzzle-part {
    position: relative;
    display: block;
    max-height: 100px;
    max-width: 100px;
    }
</style>