<script lang="ts">
    import { onMount } from "svelte";
    import Tile, { TileClass } from "./Tile.svelte";


    let board: TileClass[][] = [];
    export let rows: number = 10;
    export let cols: number = 10;
    export let minesCount: number = 40;

    const addMine = (preBoard: TileClass[][]) => {
        let row = -1;
        let col = -1;

        do {
            row = Math.floor(Math.random() * rows);
            col = Math.floor(Math.random() * cols);
        } while (preBoard[row][col].hasMine || preBoard[row][col].hasBeenVisited);

        preBoard[row][col].hasMine = true;
    }

    const fillBoard = (): void => {
        const preBoard = new Array(rows).fill(undefined).map(() =>new Array(cols).fill(undefined).map(() => new TileClass()));
        for (let i = 0; i < Math.min(minesCount, rows * cols - 1); i++) {
            addMine(preBoard);
        }
        board = preBoard;
    }

    onMount(() => fillBoard());

    const handleClick = (e: MouseEvent, i: number, j:number) => {
        console.log("click", i, j);
        board[i][j].hasBeenVisited = true;
        board = board;
    }
    const handleRightClick = (e: MouseEvent, i: number, j:number) => {
        e.preventDefault();
        console.log("rightclick", i, j);
        board[i][j].hasFlag = !board[i][j].hasFlag;
        board = board;
    };

    // $: console.table(board);
</script>

<div class="grid" style="--rows: {rows}; --cols: {cols}">
    {#each board as row, i}
        {#each row as tile, j}
            <!-- {#key tile} -->
            <Tile self={tile} row={i} col={j} on:click={(e) => handleClick(e, i, j)} on:contextmenu={(e) => handleRightClick(e, i, j)}/>
            <!-- {/key} -->
        {/each}
    {/each}
</div>

<style>
    .grid {
        display: grid;
        grid-template-rows: repeat(var(--rows), 2rem);
        grid-template-columns: repeat(var(--cols), 2rem);
    }
</style>
