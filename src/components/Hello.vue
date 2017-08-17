<template>
  <div class="hello container">
    <div class="header">
      <h1>Welcome to the PLF email signature generator.</h1>
      <p>Enter your information, copy, paste, and you're good to go.</p>
    </div>
    <div class="row">
      <div class="col">
        <h5>Enter your information here:</h5>

        <div class="form-group">
          <label>Name:</label>
          <input class="form-control" v-model="name">
        </div>

        <div class="form-group">
          <label>Title:</label>
          <input class="form-control" v-model="title">
        </div>

        <div class="form-group">
          <label>Image:</label>
          <input class="form-control" v-model="image">
        </div>

        <div class="form-group">
          <label>Phone:</label>
          <input class="form-control" v-model="phone">
        </div>

        <div class="form-group">
          <label>Cell:</label>
          <input class="form-control" v-model="cell">
        </div>
      </div>
      <div class="col">
        <h5>Copy & paste from here:</h5>
        <div id="signature" style="padding: 10px">
          <div class="copy-this" v-html="signature"></div>
        </div>
      </div>

    </div>
    <img src="assets/plf-logo.png">
  </div>
</template>

<script>
import { mjml2html } from 'mjml'
const md5 = require('js-md5')
export default {
  name: 'hello',
  data () {
    return {
      name: 'Robert L. Krauter',
      title: 'Chief Communications Officer',
      department: 'Communications Department',
      image: 'https://en.gravatar.com/avatar/c5944e489306f210199feb557d4693a7?s=200',
      email: 'RKrauter@pacificlegal.org',
      phone: '555-555-5555',
      cell: '555-555-5555',
      profile: 'http://www.pacificlegal.com/staff/robert-krauter',
      social: {
        facebook: 'http://www.facebook.com',
        twitter: 'http://www.twitter.com',
        instagram: 'http://www.instagram.com'
      },
      vars: {
        cobalt: '#0053BC',
        yellow: '#F4B73E'
      }
    }
  },
  computed: {
    socialNetworks: function() {
      var obj = this.social
      var r = ""

      for (var s in obj) {
        if(obj.hasOwnProperty(s)) {
          r += s + ":url "
        }
      }

      return r
    },
    signature: function () {
      var app = this

      const { errors, html } = mjml2html(`
        <mjml>
          <mj-body>
            <mj-container padding="0px">
              <mj-section padding="0px">
                <mj-group>
                  <mj-column width="20%" padding="0px">
                    <mj-image padding="0px 20px 0px 0px" src="${app.image}"></mj-image>
                  </mj-column>
                  <mj-column width="70%">
                    <mj-text
                      font-size="14px"
                      font-family="Roboto, Helvetica, sans-serif"
                      align="left"
                      padding="10px 0px"
                      >
                      <span style="font-weight:bold">${app.name} | </span> <span>${app.title}</span>
                      <br/ >
                      <span style="font-style:italic">${app.department}</span>
                      <br/>
                      <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="http://www.pacificlegal.org">Pacific Legal Foundation</a></span>
                    </mj-text>
                    <mj-text
                      font-size="14px"
                      font-family="Roboto, Helvetica, sans-serif"
                      align="left"
                      padding="0px 0px 10px"
                      >
                      <span style="font-color:black">email:</span> <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="mailto:${app.email}">${app.email.toLowerCase()}</a></span>
                      <br/ >
                      <span style="font-color:black">phone:</span> <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="tel:${app.phone}">${app.phone}</a></span>
                      <br/ >
                    </mj-text>
                      <mj-social
                        font-family="Roboto, Helvetica, sans-serif"
                        text-mode="false"
                        align="left"
                        inner-padding="5px"
                        padding="0px"
                        display="${app.socialNetworks}"
                        google-icon-color="${app.vars.yellow}"
                        facebook-icon-color="${app.vars.yellow}"
                        twitter-icon-color="${app.vars.yellow}"
                        instagram-icon-color="${app.vars.yellow}"

                        google-href="${app.social.google}"
                        facebook-href="${app.social.facebook}"
                        twitter-href="${app.social.twitter}"
                        instagram-href="${app.social.instagram}"
                      />
                    <mj-image padding="0px" src="/assets/plf-logo.png"></mj-image>
                  </mj-column>
                </mj-group>
              </mj-section>
            </mj-container>
          </mj-body>
        </mjml>

      `)

      return html
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass">
  .container
    margin-top: 40px
  .header
    margin-bottom: 40px
  #signature
    border: 2px solid lighten(#4A4A4A, 50%)
</style>
