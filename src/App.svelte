<script>
    import Quiz from "./Quiz.svelte";
</script>

<style>
    :root {
        --primary: #256EFF;
        --secondary: #DDFC74;
        --dark: #232528;
        --header-font: 'Rubik Doodle Shadow', system-ui;
    }

    :global(body) {
        background: var(--dark);
        font-family: sans-serif;
    }

    h1 {
        color: white;
        text-align: center;
        font-family: var(--header-font);
        font-size: 4rem;
    }

    div {
        display: flex;
        color: white;
        border-radius: 15px;
        padding: 40px;
    }
</style>


<h1>Music Quiz!</h1>

<div>
  <Quiz/>
</div>




