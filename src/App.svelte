<script>
  import Editor from "./Editor.svelte";

  let data = {
    time: 1597183675054,
    blocks: [
      {
        type: "paragraph",
        data: {
          text: "WELCOME TO EDITOR JS!",
        },
      },
      {
        type: "paragraph",
        data: {
          text: "Svelte is truly, truly awesome!",
        },
      },
    ],
    version: "2.18.0",
  };

  let handleSave = (e) => {
    console.log(e.detail);
  };
</script>

<Editor on:save={handleSave} {data} />
