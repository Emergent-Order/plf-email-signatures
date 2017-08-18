<template>
  <div class="hello container">
    <div class="header">
      <h1>Welcome to the PLF email signature generator.</h1>
      <p>Enter your information, copy, paste, and you're good to go.</p>
    </div>
    <div class="row">
      <div class="col-12 col-md-5">
        <h5>Enter your information here:</h5>
        <div class="form-wrapper">
        <div class="form-group">
          <label>Image:</label>
          <input class="form-control" v-model="image">
           <small id="emailHelp" class="form-text text-muted">Choose a publicly accessible url, like your Gravatar image, or an image on the new website.</small>
        </div>

        <div class="form-group">
          <label>Name:</label>
          <input class="form-control" v-model="name">
        </div>

        <div class="form-group">
          <label>Title:</label>
          <input class="form-control" v-model="title">
        </div>

        <div class="form-group">
          <label>Department:</label>
          <input class="form-control" v-model="department">
        </div>

        <h5 class="section-break">Contact information:</h5>
        <div v-for="(item, key) in contact" class="form-group">
          <label>{{ key | capitalize }}:</label>
          <input class="form-control" v-model="contact[key]">
        </div>

        <h5 class="section-break">Social accounts:</h5>
        <div v-for="(item, key) in social" class="form-group">
          <label>{{ key | capitalize }}:</label>
          <input class="form-control" v-model="social[key]">
        </div>
      </div>
      </div>
      <div class="col-12 col-md-6 ml-auto signature-renderer">
        <h5>Copy & paste from here:</h5>
        <div id="signature" style="padding: 10px">
          <div id="copy-this" v-html="signature"></div>
        </div>
        <div class="wrapper">
          <button class="btn" @click="selectText">Copy to Clipboard</button>
          <p v-if="successtext">{{ successtext }}</p>
        </div>
      </div>

    </div>
  </div>
</template>

<script>
import { mjml2html } from 'mjml'
const cheerio = require('cheerio')

export default {
  name: 'hello',
  data () {
    return {
      name: 'Your Name Here',
      title: 'Your title here',
      department: 'You Can List A Department',
      // image: 'https://en.gravatar.com/avatar/c5944e489306f210199feb557d4693a7?s=200',
      image: '',
      contact: {
        email: 'your.email@address.com',
        phone: '555-555-5555',
        cell: '555-555-5555',
        profile: 'http://www.pacificlegal.com/staff/this-feature-coming-soon',
        fax: '(916) 419-7111'
      },
      social: {
        facebook: 'http://www.facebook.com/PacificLegalFoundation/',
        twitter: 'https://twitter.com/PacificLegal',
        instagram: 'http://instagram.com/pacific_legal_foundation'
      },
      vars: {
        cobalt: '#0053BC',
        yellow: '#F4B73E',
        navy: '#20334B'
      },
      successtext: ''
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
    contactInfo: function() {
      var app = this
      var obj = this.contact
      var r = ""

      for (var s in obj) {
        if(obj.hasOwnProperty(s) && obj[s]) {
          r += `<span style="font-color:black">${s}:</span> <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="mailto:${obj[s]}">${obj[s].toLowerCase()}</a></span><br/>`
        }
      }

      return r


      // <span style="font-color:black">email:</span> <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="mailto:${app.email}">${app.email.toLowerCase()}</a></span>
      // <br/ >
      // <span style="font-color:black">phone:</span> <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="tel:${app.phone}">${app.phone}</a></span>
      // <br/ >
    },
    photo: function () {
      var app = this
      if (app.image) {
        return `<mj-column width="100px" padding="0px"><mj-image padding="0px 20px 0px 0px" src="${app.image}"></mj-image></mj-column>`
      } else {
        return ''
      }
    },
    socialAll: function () {
      var app = this
      var obj = this.social
      var r = ""
      for (var s in obj) {
        if(obj.hasOwnProperty(s) && obj[s]) {
          r += `<mj-column><mj-button href="${obj[s]}"><mj-image src="https://github.com/Emergent-Order/plf-email-signatures/blob/master/assets/${s}-icon.png?raw=true"></mj-image></mj-button></mj-column>`
        }
      }

      return r
    },
    signature: function () {
      var app = this

      const { errors, html } = mjml2html(`
        <mjml>
          <mj-head>
            <mj-attributes>
              <mj-text padding="0" />
              <mj-all font-family="Roboto, Helvetica, Arial, sans-serif" />
              <mj-text align="left"/>
            </mj-attributes>
          </mj-head>
          <mj-body >
            <mj-container align="left">
              <mj-section text-align="left" padding="0px" >
                <mj-group style="width: 100%">
                    ${app.photo}
                  <mj-column >
                    <mj-text
                      font-size="18px"
                      align="left"
                      padding="10px 0px 0px 0px"
                      color="${app.vars.navy}"
                      >
                      <span style="font-weight:bold">${app.name} | </span> <span>${app.title}</span>
                    </mj-text>
                    <mj-text
                      font-size="14px"
                      align="left"
                      padding="0px 0px 10px 0px"
                      >
                      <span style="font-style:italic">${app.department}</span>
                      <br/>
                      <span><a style="color: ${app.vars.cobalt}; font-weight: bold" href="http://www.pacificlegal.org">Pacific Legal Foundation</a></span>
                    </mj-text>
                    <mj-text
                      font-size="14px"
                      align="left"
                      padding="0px 0px 10px"
                      line-height="20px"
                      >
                      ${app.contactInfo}
                    </mj-text>
                    <mj-text
                      font-size="14px"
                      align="left"
                      padding="0px 0px 10px"
                      line-height="20px"
                      >
                      <span>930 G Street | Sacramento, CA 95814</span>
                    </mj-text>
                    <mj-group>
                      <mj-column>


                      <mj-social
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
                    <mj-image padding="20px 0px 10px" width="200px" align="left" src="https://github.com/Emergent-Order/plf-email-signatures/blob/master/assets/plf-logo.png?raw=true"></mj-image>
                  </mj-column>
                </mj-group>
              </mj-section>
            </mj-container>
          </mj-body>
        </mjml>

      `)

      const $ = cheerio.load(html)
      var firstTable = $('table')[0]

      $(firstTable).attr('style', "box-sizing: inherit; border-collapse: collapse; color: rgb(33, 37, 41); font-family: -apple-system, system-ui, 'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif; orphans: 2; widows: 2; font-size: 0px;")
      $(firstTable).css('width', '100%')
      $(firstTable).attr('align', '')

      return $.html()
    }
  },
  methods: {
    selectText: function () {

      if (document.selection) {
          var content = document.getElementsTagName('table')[0]
          var range = document.body.createTextRange()
          range.moveToElementText(content)
          range.select();

          // var controlRange = document.body.createControlRange();
          // controlRange.addElement(content);
          document.execCommand('Copy');
      } else if (window.getSelection) {
          var content = document.getElementsByTagName('table')[0]
          var range = document.createRange();
          range.selectNode(content);
          window.getSelection().removeAllRanges();
          window.getSelection().addRange(range);

          // var controlRange = document.body.createControlRange();
          // controlRange.addElement(content);
          document.execCommand('Copy');
      }

      this.successtext = "Successfully copied!"
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style lang="sass">
  body
    background-color: #F2F2F2
  .container
    margin-top: 40px
    margin-bottom: 40px
  .header
    margin-bottom: 40px
  .form-control
    border-radius: 0px
  #signature
    background-color: white
  .btn
    background-color: #002997
    border-radius: 0px
    color: white
    margin-top: 20px
    font-size: 12px
    text-transform: uppercase
    letter-spacing: 1.5px
    cursor: pointer
    &:hover
      background-color: darken(#002997, 5%)
  h5
    margin-bottom: 1rem
  .form-wrapper
    background-color: #F2F2F2
    // padding: 30px
  .section-break
    margin-top: 50px
  .signature-renderer
    padding: 10px

</style>
