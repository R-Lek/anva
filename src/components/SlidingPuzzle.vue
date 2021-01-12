<template>
    <div class="puzzle-container">
        <div class="anva-preview">
            <img class="anva-logo" src="/assets/anva_logo.png">
        </div>
        <div class="anva-puzzle">
            <table id="puzzle">
                <puzzle-piece :puzzlePieces="puzzlePieces" @piece-id="movePiece" />    
            </table>
            <img id="hidden-logo" src="/assets/anva_logo.png">
        </div>
    </div>
</template>

<script>
import PuzzlePiece from '@/components/PuzzlePiece.vue'

export default {
    name: 'SlidingPuzzle',
    components: { PuzzlePiece },
        data() {
            return {
                puzzlePieces: [],
                emptySlot: 16,
                previousPieceKey: 15,
                previousPieceValue: 16
            };
        },
        methods: {
            randomizePuzzle() {
                while (this.puzzlePieces.length < 15) {
                    var r = Math.floor(Math.random() * 15) + 1;
                    if (this.puzzlePieces.indexOf(r) === -1) {
                        this.puzzlePieces.push(r);
                    }
                }

                // 'Shortcut' for checking if solution works
                // for (let i = 1; i < 16; i++) {
                //     this.puzzlePieces.push(i);
                // }

                // No image is assiged to the last puzzle piece, but the 16th piece is included in the puzzlePieces array
                this.puzzlePieces.push(16);
            },
            puzzleFinished() { 
                for (let i = 0; i < this.puzzlePieces.length; i++) { 
                    if (this.puzzlePieces[i] === i + 1) {
                        continue;
                    } else {
                        return;
                    }
                }
                return true; 
            },
            movePiece(piece) {
                // If the 'selected puzzle piece' is adjacent to the 'previous puzzle piece', move the 'selected puzzle piece'
                if (this.previousPieceKey === (piece - 1) + 1 || this.previousPieceKey === (piece - 1) - 1 || this.previousPieceKey === (piece - 1) + 4 || this.previousPieceKey === (piece - 1) - 4) {
                    
                    // 'Previous puzzle piece' is replaced with the 'selected puzzle piece'
                    this.selectedPieceValue = this.puzzlePieces[Number(piece) - 1];
                    this.puzzlePieces.splice(this.previousPieceKey, 1, this.selectedPieceValue);

                    // 'Selected puzzle piece' is replaced with the 'empty slot'
                    this.puzzlePieces.splice(piece - 1, 1, this.emptySlot);

                    // The current 'selected puzzle piece' is stored in the end as the 'previous puzzle piece' for the next move action
                    this.previousPieceKey = Number(piece) - 1;
                    this.previousPieceValue = this.puzzlePieces[Number(piece) - 1];
                }

                // Check if puzzle is finished/complete
                if (this.puzzleFinished() === true) {
                    document.getElementById('puzzle').style.display = 'none';
                    document.getElementById('hidden-logo').style.display = 'block';
                }
            }
        },
        mounted() {
            this.randomizePuzzle();
        }

    }
</script>

<style scoped>
    .puzzle-container {
        display: flex;
        flex-wrap: wrap-reverse;
    }

    #hidden-logo {
        display: none;
        height: 500px;
        width: 500px;
    }

    .anva-preview, 
    .anva-puzzle {
        height: 400px;
        width: 400px;
    }

    .anva-logo {
        height: 300px;
        width: 300px;
    }

    #puzzle {
        border-collapse: collapse;
        border-spacing: 0;
    }
</style>
