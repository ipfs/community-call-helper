<template>
    <div class="hompage">
        <h1 class="center community-call-title">Community Call Utils</h1>
        <main class="pa4 black-80">
            <div class="measure center">
                <section class="call-detail">
                    <fieldset id="a" class="ba b--transparent ph0 mh0">
                    <legend class="f4 fw6 ph0 mh0">Fill In Community Call Details 📝</legend>
                    <div class="mt3">
                        <label class="db fw6 lh-copy f6" for="textfield">Endeavor</label>
                        <input class="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="endeavor">
                    </div>
                    <div class="mv3">
                        <label class="db fw6 lh-copy f6" for="textfield">Moderator</label>
                        <input class="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="moderator">
                    </div>
                    <div class="mv3">
                        <label class="db fw6 lh-copy f6" for="textfield">Notetaker</label>
                        <input class="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="noteTaker">
                    </div>
                    <div class="mv3">
                        <label class="db fw6 lh-copy f6" for="textfield">Zoom Link</label>
                        <input class="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="zoomLink">
                    </div>

                    </fieldset>
                </section>
                <section class="call-announcement">
                    <fieldset id="a" class="ba b--transparent ph0 mh0">
                    <legend class="f4 fw6 ph0 mh0">Broadcast Community Call 🎉🎊🎉 </legend>
                    <div class="mt3">
                        <label class="db fw6 lh-copy f6" for="sync-date">Date of Community Sync</label>
                        <input class="pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="sync-date"  id="sync-date" v-model="syncDate">
                    </div>
                    <div class="mv3">
                        <label class="db fw6 lh-copy f6" for="textfield">Presenter</label>
                        <input class="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="presenter">
                    </div>
                    <div class="mv3">
                        <label class="db fw6 lh-copy f6" for="textfield">Topic</label>
                        <input class="b pa2 input-reset ba bg-transparent hover-bg-black hover-white w-100" type="textfield" name="textfield"  id="textfield" v-model="topic">
                    </div>
                    </fieldset>
                </section>    
                    <div class="input-button">
                        <input class="b ph3 pv2 input-reset ba b--black bg-transparent grow pointer f6 dib popup" type="submit" value="Submit" @click="createPost()">
                    </div>
            </div>
        </main>
    </div>
</template>
<script>
import config from '../../config.json'
const template = (data) => `### IPFS  Weekly Call Details
Please add your agenda items before the call.

Endeavour      | Moderator            | Notetaker | Time (PST - UTC - CET) | Pad
:------------: | :-------------: | :-------: | :--------------------: | :----:
${data.endeavor} | ${data.moderator}    | ${data.noteTaker}  | 9:00 **17:00** 18:00  | [agenda and notes](https://docs.google.com/document/d/1WHyIZhBo2eEgYXlZ5HLHg6a6ZWTH3tV848sWkYBJjJA/edit)


Presenter: ${data.presenter}

Topic: ${data.topic}

Zoom link for joining the call:  ${data.zoomLink}
`
export default {
  name: 'HomePage',
  data: () => ({
    endeavor: template(),
    moderator: this.moderator,
    noteTaker: this.noteTaker,
    zoomLink: this.zoomLink,
    syncDate: null,
    presenter: this.presenter,
    topic: this.topic,
    error: false
  }),
  methods: {
    async createPost() {
      let url = `https://api.github.com/repos/ipfs/team-mgmt/issues?access_token=${config.token}`
      const rawResponse = await fetch(url, {
        method: 'POST',
        headers: {
        'Accept': 'application/json',
        'Content-Type': 'application/json'
        },
        body: JSON.stringify({
          title: `IPFS Weekly Call ${this.syncDate} ⚡️📞`,
          body: template({ endeavor: this.endeavor, noteTaker: this.noteTaker, zoomLink: this.zoomLink, moderator: this.moderator, presenter: this.presenter, topic: this.topic,})
        })
      })
      const content = await rawResponse.json();
      console.log(content)
    }
  }
}
</script>
<style>
    h1 {
        text-align: center;
    }
    .community-call-title{
        text-decoration: underline;
    }
</style>
