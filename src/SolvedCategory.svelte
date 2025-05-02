<script>
    const { title, cards, index } = $props();
</script>

<style>
    .solved-category {
        display: flex;
        flex-direction: column;
        align-items: center;
        justify-content: center;
        width: 100%;
        background-color: aliceblue;
        height: 100px;
        font-weight: 600;
        font-size: 1em;
        border-radius: 5px;
        text-transform: uppercase;
        margin-bottom: 10px;
    }

    .difficulty-0 {
        background-color: #eeedbe;
    }

    .difficulty-1 {
        background-color: #abe0b4;
    }

    .difficulty-2 {
        background-color: #9fcfdd;
    }

    .difficulty-3 {
        background-color: #cf9dd1;
    }
</style>

<div
    class={["solved-category", `difficulty-${index}`]}
>
    <h4 style="margin: 0px">{title}</h4>
    <h5 style="margin: 0px">{cards.join(', ')}</h5>
</div>
