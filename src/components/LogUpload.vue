<template>
  <div class="main">
    <div class="drag" id="drag">
      <h1>Upload your files</h1>
      <p>Files should have to be extracted before uploading</p>
      <div class="filedrop" @drop.prevent="fireSelection">
        <div class="dragarea" id="draga">
          <input
            type="file"
            class="hide"
            id="multiple-file"
            @change="selectMultipleFiles"
            ref="files"
            multiple="multiple"
          />
          <img src="../assets/folder-open.svg" id="folder" />
          <p>Drag and drop your logs here</p>
        </div>
      </div>
      <div class="btns" v-if="filesdropped">
        <button id="uploadLogs" @click="sendMultipleFile">Upload logs</button>
        <button id="analyzeLogs" @click="analyzeLogs">Analyze</button>
      </div>
      <div class="progress" v-if="isuploading">
        <div class="uploading">Uploading Files</div>
        <div class="file-bar">
          <img src="../assets/log-format.svg" id="logformat" />
          <div class="progression">
            <div class="count">
              <div class="bold">Uploading {{ filecount }} file(s).</div>
              <div>{{ progress }}%</div>
            </div>
            <progress class="pbar" :value="progress" max="100">
              {{ progress }}</progress
            >
          </div>
          <img src="../assets/checked.svg" id="completed" v-if="isuploaded" />
        </div>
      </div>
    </div>
  </div>
</template>
<script>
import axios from "axios";
import _ from "lodash";
export default {
  name: "LogUpload",
  data() {
    return {
      files: [],
      ticket: "",
      progress: 0,
      isuploading: false,
      filecount: 0,
      filesdropped: false,
      isuploaded: false,
    };
  },
  methods: {
    upload() {
      console.log("clicked");
    },
    analyze() {
      console.log("clicked c");
    },
    selectMultipleFiles() {
      const files = this.$refs.files.files;
      // this.ticket = this.ticket;
      this.files = [...this.files, ...files];
      this.filesdropped = true;
      this.isuploading = true;
      this.filecount = this.files.length;
    },
    async sendMultipleFile() {
      let uri = window.location.href.split("?");
      if (uri.length == 2) {
        this.ticket = uri[1].replaceAll("%22", "").substr(4);
      }
      console.log(this.ticket);
      const formdata = new FormData();
      formdata.append("ticket", this.ticket);
      _.forEach(this.files, (file) => {
        formdata.append("files", file);
      });
      try {
        this.isuploading = true;

        await axios
          .post("/multiple", formdata, {
            onUploadProgress: (e) =>
              (this.progress = Math.round((e.loaded * 100) / e.total)),
          })
          .then((response) => {
            this.isuploaded = true;
            console.log(response);
          });
      } catch (err) {
        console.log(err);
        this.isuploading = false;
      }
    },
    trFileTree(item, path) {
      path = path || "";
      var self = this;
      if (item.isFile) {
        // Get file
        item.file(function(file) {
          // this.files = [...this.files, ...file]
          self.files.push(file);
          //   console.log(file);
          self.filecount = self.files.length;
        });
      } else if (item.isDirectory) {
        // Get folder contents
        var dirReader = item.createReader();
        dirReader.readEntries(function(entries) {
          for (var i = 0; i < entries.length; i++) {
            // let self = this
            self.trFileTree(entries[i], path + item.name + "/");
          }
        });
      }
    },
    fireSelection(event) {
      //   console.log("drag invoked");
      this.filesdropped = true;
      this.isuploading = true;

      document.getElementById("drag").classList.add("grow");

      var items = event.dataTransfer.items;
      for (var i = 0; i < items.length; i++) {
        // webkitGetAsEntry is where the magic happens
        var item = items[i].webkitGetAsEntry();
        if (item) {
          this.trFileTree(item);
        }
      }
    },
    async analyzeLogs() {
      let uri = window.location.href.split("?");
      if (uri.length == 2) {
        this.ticket = uri[1].replaceAll("%22", "").substr(4);
      }
      // console.log(this.ticket)
      // const formdata = new FormData();
      // let ticket = this.ticket;
      console.log(this.ticket);
      // formdata.append("ticket",this.ticket)
      await axios
        .get("/read/" + this.ticket, {
          params: {
            ticket: this.ticket,
          },
        })
        .then((response) => {
          console.log(response);
        });
    },
  },
};
</script>
<style>
.dragarea {
  width: 100%;
}

.hide {
  opacity: 0;
  z-index: 3;
  position: absolute;
  width: 100%;
  height: 200px;
  cursor: pointer;
}
.filedrop {
  margin: 20px;
}
</style>
