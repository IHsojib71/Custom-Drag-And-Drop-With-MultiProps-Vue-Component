<template>
    <div>
        <v-label v-if="label" class="mb-2">{{label}} <span v-if="mandatoryFieldSign" class="red--text"><strong>*</strong></span></v-label>
        <div v-if="previousFiles.length" class="mb-2">
            <span class="grey--text">Previous Files: </span>
            <v-row>
                <v-col
                    v-for="(image, index) in previousFiles"
                    :key="index"
                    class="d-flex child-flex col-6 col-md-4"
                >
                    <v-img
                        :src="image"
                        :lazy-src="image"
                        aspect-ratio="1.7"
                        class="grey lighten-2 rounded-lg"
                    >
                        <template v-slot:placeholder>
                            <v-row
                                class="fill-height ma-0"
                                align="center"
                                justify="center"
                            >
                                <v-progress-circular
                                    indeterminate
                                    color="grey lighten-5"
                                ></v-progress-circular>
                            </v-row>
                        </template>
                    </v-img>
                </v-col>
            </v-row>
        </div>
        <file-pond
            class="mb-0 pb-0"
            name="filepond"
            ref="pond"
            label-idle='Drag & Drop your files or <span class="filepond--label-action"> Browse </span>'
            :allow-multiple="allowMultiple"
            :maxFiles="maximumFiles"
            :accepted-file-types="fileTypes"
            :files="myFiles"
            @init="handleFilePondInit"
            imagePreviewHeight="150"
            :allowImagePreview="allowPreview"
            instant-upload="false"
            @updatefiles="update"
        />
        <div
            style="margin-top: -15px!important;"
             v-if="errorMessages">
             <span style="color: red;">{{ errorMessages}}</span>
        </div>

    </div>
</template>

<script>
// Import Vue FilePond
import vueFilePond from 'vue-filepond';

// Import FilePond styles
import 'filepond/dist/filepond.min.css';

// Import FilePond plugins
// Please note that you need to install these plugins separately

// Import image preview plugin styles
import 'filepond-plugin-image-preview/dist/filepond-plugin-image-preview.min.css';

// Import image preview and file type validation plugins
import FilePondPluginFileValidateType from 'filepond-plugin-file-validate-type';
import FilePondPluginImagePreview from 'filepond-plugin-image-preview';
import FilePondPluginImageExifOrientation from 'filepond-plugin-image-exif-orientation';
import FilePondPluginImageCrop from 'filepond-plugin-image-crop';
// Create component
const FilePond = vueFilePond(FilePondPluginFileValidateType,FilePondPluginImagePreview, FilePondPluginImageExifOrientation,FilePondPluginImageCrop);

export default {
    props:{
      allowPreview : Boolean,
      allowMultiple : Boolean,
      maximumFiles : Number,
      errorMessages : String,
      fileTypes : String,
      previousFiles : Array,
      label :String,
      mandatoryFieldSign : Boolean
    },
    components: {
        FilePond
    },
    data() {
        return { myFiles: [] };
    },
    watch:{
        myFiles(){
            this.allowMultiple===false ?  this.$emit("files", this.myFiles[0]) : this.$emit("files", this.myFiles);
        }
    },
    methods: {
        handleFilePondInit: function() {
            // console.log('FilePond has initialized');
            // FilePond instance methods are available on `this.$refs.pond`
        },
        update(files){
                this.myFiles = files.map(files => files.file);
    },

    },
};
</script>

<style>

.filepond--item {
    width: calc(50% - 0.5em);
}

.filepond--credits {
    display: none;
}
</style>
