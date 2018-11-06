[Retour](../js.md)

# JavaScript - MediaChooser

## JS
```
$(function() {
    $("#mediaChooseImage").rsMediaChooser({
        multiple: false,
        allowedMedia: ['picture', 'video', 'video_ext', 'audio', 'document', 'folder'],
        doneFunction: function (data) {
            // store the id in a input field
            $('#imageName').val(data.name);
            $('#imageId').val(data.id);
        }
    });
});
```

## HTML
```
<div class="form-group">
    <label class="control-label" for="thumbnail">Image Thumbnail</label>
    <div class="input-group">
        <?php
        $media = new Media($id);
    
        $name = $media->name;
        $id = $media->id;
        ?>
        <input value="<?php echo $name ?>" id="imageName" name="imageName" placeholder="Image" class="form-control input-md" type="text">
        <input value="<?php echo $id ?>" id="imageId" name="imageId" type="hidden">
        <span id="mediaChooseImage" class="input-group-addon btn btn-primary">Choisir</span>
    </div>
    <p class="help-block"> The image that is displayed in the list of items</p>
</div>
```