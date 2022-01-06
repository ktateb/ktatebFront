<template>
  <div>
    <v-card class="bodyCard" outlined color="transparent">
      <!-- <div class="noMessages">
      <p>!no messages yet</p>
    </div> -->
      <ul class="chat">
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>

        <br style="clear: both" />
        <li>
          <!-- to me -->
          <div class="rightChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to me -->
          <div class="rightChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
        <li>
          <!-- to user -->
          <div class="leftChat">
            <div class="chatBody">
              <p>مرحبا كيفك شو الاخبار</p>
              <div class="header">
                <small class="text-muted">
                  <span class="fa fa-clock-o float-left">22/12/2020</span>
                </small>
                <br />
              </div>
            </div>
          </div>
        </li>
        <br style="clear: both" />
      </ul>
    </v-card>
    <div>
      <v-container fluid class="submitForm">
        <v-row>
          <v-textarea
            rounded
            class="px-2"
            auto-grow
            filled
            color="#6FBBAC"
            rows="1"
            style="width: 50%; display: inline-block; vertical-align: middle"
          ></v-textarea>
          <!-- <i class="mdi mdi-send sendIcon"></i> -->
          <div class="text-center">
            <v-btn rounded color="primary my-2 px-2 ml-2" dark> ارسال </v-btn>
          </div>
        </v-row>
      </v-container>
    </div>
  </div>
</template>

<style scoped>
.noMessages {
  padding-top: 220px;
  padding-bottom: 220px;
  text-align: center;
}
.leftChat {
  background-color: #6fbbac;
  width: auto;
  height: auto;
  float: left;
  color: white;
  padding: 5px;
  margin: 10px;
  border-radius: 6px;
  border: 1px solid #ccc;
}
.rightChat {
  background-color: #6fbbac;
  width: auto;
  height: auto;
  float: right;
  color: white;
  padding: 5px;
  margin: 10px;
  border-radius: 6px;
  border: 1px solid #ccc;
}
.sendIcon {
  font-size: 30px;
}
.bodyCard {
  overflow-y: scroll;
  height: 385px;
  border: 0 solid;
}
.submitForm {
  bottom: 0;
  position: relative;
}
</style>


