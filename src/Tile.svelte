<script context="module" lang="ts">
    export class TileClass {
  constructor(
    public hasMine: boolean = false,
    public hasBeenVisited: boolean = false,
    public hasFlag: boolean = false
  ) {}
}

</script>

<script lang="ts">
    export let self: TileClass;
    export let row: number;
    export let col: number;

    // $: console.log(self);

    const dark = (row + col) % 2 != 0;
    $: backgroundColor = (() => {
        if (!self.hasBeenVisited && dark) return "#049409";
        if (!self.hasBeenVisited) return "#00cf07";
        if (dark) return "#555";
        return "#EEE";
    })();
</script>

<div class="tile" style:background-color={self.hasFlag ? "red" : backgroundColor} on:click on:contextmenu></div>

<style>
    .tile {
        
    }
</style>
