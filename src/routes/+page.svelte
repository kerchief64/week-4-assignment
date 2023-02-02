<script>
/*
 __          __              _             
 \ \        / /             (_)            
  \ \  /\  / /_ _ _ __ _ __  _ _ __   __ _ 
   \ \/  \/ / _` | '__| '_ \| | '_ \ / _` |
    \  /\  / (_| | |  | | | | | | | | (_| |
     \/  \/ \__,_|_|  |_| |_|_|_| |_|\__, |
                                      __/ |
                                     |___/ 

If your get the following error, ignore it and refresh the page!!!!

> [HMR][Svelte] Failed to rerender <+page>
> TypeError: node.parent is undefined

*/

	import ActionSelect from "./ActionSelect.svelte";
  import AddImage from "./AddImage.svelte";
	import CustomImage from "./CustomImage.svelte";

  let selectedEditOption;
  let galleryImages = [];
  let files;

  // Turn a basic FileList into a special array of Objects.
  // Each Object has:
    // width: Integer
    // height: Integer
    // image: File Object
    // selected: Boolean
  function createImageObject(files) {
    // If no files have been added into our file buffer, return null and do nothing
    if (!files) return null;

    // Copy our existing images into the new Object so we don't lose them
    let imgObject = [...galleryImages];

    // Convert our FileList into an array of File Objects
    Array.from(files).forEach(function (file) {
      // Push our custom Object for each file
      imgObject.push({
        width: 500,
        height: 500,
        image: file,
        selected: false,
      });
    })

    // return the newly created array
    return imgObject;
  }

  // Just to help with debugging
  $: console.log("Image Gallery: ", galleryImages);
  $: console.log("Selected Edit Option: ", selectedEditOption);
  
  // Handle our button press depending on the edit option selected
  function handleEditSelect() {
    switch (selectedEditOption.id) {
      case 2:
        // If you pressed "add" without choosing at least 1 file first, return and do nothing
        if (files === undefined) return;

        // Create a new array of files with both the old and new
        galleryImages = createImageObject(files);
        break;
      case 3:
        /* 

        [Challenge 1]:
        WARNING: DO CHALLENGE 2, 3, AND 4 BEFORE STARTING THIS ONE.
        Reassign and Filter through the `galleryImages` so that only images not selected for deletion are left.

        */

        galleryImages = galleryImages.filter(x => x.selected === false)
        console.log("Remove button was pushed");
        break;
    }
    // Clear out the uploaded files if there are any
    files = undefined;
  }


</script>

<h1>Week 4 Assignment</h1>
<h2>Photo Gallery App</h2>

  <!-- 
    [Challenge 2]:
    Bind the `selected` prop inside the ActionSelect component to `selectedEditOption`
  -->
  <ActionSelect bind:selected={selectedEditOption} on:submit="{handleEditSelect}"/>

<!-- 
  Make sure `sellectedEditOption` is defined before doing anything
-->
{#if selectedEditOption !== undefined}

  <!-- if the "Add Images" action is selected, show the AddImage component -->
  {#if selectedEditOption.id === 2}
    <!-- 
      [Challenge 3]:
      Figure out why this bind isn't working
    -->
    <AddImage bind:files />
    <br>
    <br>
  {/if}

    <!-- If there are `galleryImages` to display, continue -->
    {#if galleryImages}
      {#each galleryImages as myImg}
        <!-- if the "Remove Images" action is selected, show the check boxes -->
        {#if selectedEditOption.id === 3}
          <br>
          <br>
            <!-- 
              [Challenge 4]:
              Bind each checkbox and it's sibling image (myImg) together via the `selected` object field
              which is held by each myImg 
            -->
          <input type="checkbox" bind:checked={myImg.selected} style="width: 50px; height: 50px;">
          <br>
        {/if}
        <!-- Display the image -->
        <!-- 
          [Challenge 5]:
          Instead of using a raw image, make a custom image component called "CustomImage"
          This component will hold the exact same html below, 
          but you will be passing the following to the component through props:

          1: `src`
          2: `width`
          3: `height`

          Once this is working, use the `onDestroy` life cycle hook in the "CustomImage" component.
          Inside the life cycle hook, run the following code: `console.log("An image was deleted")`
        -->
        <CustomImage src={URL.createObjectURL(myImg.image)} width={myImg.width} height={myImg.height} selected={myImg.selected} />
         <!-- 
              !! [Super Challenge] !!
              Use the `.markedForDelete` class on the "CustomImage" components.
              However, only activate this class if the image has been marked for deletion (the checkbox has been checked)
          -->
      {/each}
    {/if}
{/if}

