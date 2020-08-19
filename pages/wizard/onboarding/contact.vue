<template>
  <div class="section">
    <h2>Kontaktdaten</h2>
    <!--<form class="form" @submit.prevent="updateUser">-->
      <!--
      <div class="form-item">
        <span class="label">Vorname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.firstName" name="" id=""/>
      </div>
      <div class="form-item">
        <span class="label">Nachname</span>
        <input class="input-text" disabled type="text" v-model="user.profile.lastName" name="" id=""/>
      </div>
      -->
    <div v-bind:class="{ sectionFlex : imageData != null}">
      <div>
        <div class="form-item">
          <div class="info-block">
            <span class="label">Foto für deinen Mietgliedsausweis</span>
            <label for="picture" class="picture-file-upload">
              Datei hochladen
              <input type="file" v-on:change="getPicture" ref="picture" id="picture">
            </label>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">Geburtsdatum</span>
              <input class="input-text" type="date" v-model="user.profile.birthdate" name="" id=""/>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">Telefon</span>
              <input class="input-text" type="text" v-model="user.profile.phone" name="" id="" placeholder="+43"/>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">Adresse</span>
              <input class="input-text" type="text" v-model="user.profile.address" name="" id=""/>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label"></span>
              <input class="input-text" type="text" v-model="user.profile.address2" name="" id=""/>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">PLZ</span>
              <div class="zipError">
                <div class="option" v-bind:class="{missing: user.errors.zip === false}">
                  <input class="input-text" type="text" v-model="user.profile.zip" name="" id=""/>
                </div>
                <p v-if="user.errors.zip === false">Bitte eine gültige PLZ angeben</p>
              </div>
            </div>
          </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">Stadt</span>
              <input class="input-text" type="text" v-model="user.profile.city" name="" id=""/>
            </div>
          </div>
        <div class="form-item">
          <div class="info-block">
            <span class="label">Land</span>
            <select class="input-text select" v-model="user.profile.country">
              <option value="AF" v-model="user.profile.country">Afghanistan</option>
              <option value="EG" v-model="user.profile.country">Ägypten</option>
              <option value="AL" v-model="user.profile.country">Albanien</option>
              <option value="DZ" v-model="user.profile.country">Algerien</option>
              <option value="US" v-model="user.profile.country">Amerika</option>
              <option value="AD" v-model="user.profile.country">Andorra</option>
              <option value="AG" v-model="user.profile.country">Antigua und Barbuda</option>
              <option value="GQ" v-model="user.profile.country">Äquatorialguinea</option>
              <option value="AE" v-model="user.profile.country">Arabische Emirate</option>
              <option value="AR" v-model="user.profile.country">Argentinien</option>
              <option value="AM" v-model="user.profile.country">Armenien</option>
              <option value="AW" v-model="user.profile.country">Aruba</option>
              <option value="AU" v-model="user.profile.country">Australien</option>
              <option value="AZ" v-model="user.profile.country">Azerbaijan</option>
              <option value="BS" v-model="user.profile.country">Bahamas</option>
              <option value="BH" v-model="user.profile.country">Bahrain</option>
              <option value="BD" v-model="user.profile.country">Bangladesh</option>
              <option value="BB" v-model="user.profile.country">Barbados</option>
              <option value="BY" v-model="user.profile.country">Belarus</option>
              <option value="BE" v-model="user.profile.country">Belgien</option>
              <option value="BZ" v-model="user.profile.country">Belize</option>
              <option value="BJ" v-model="user.profile.country">Benin</option>
              <option value="BM" v-model="user.profile.country">Bermuda</option>
              <option value="BT" v-model="user.profile.country">Bhutan</option>
              <option value="BO" v-model="user.profile.country">Bolivien</option>
              <option value="BA" v-model="user.profile.country">Bosnien und Herzegowina</option>
              <option value="BW" v-model="user.profile.country">Botswana</option>
              <option value="BR" v-model="user.profile.country">Brazilien</option>
              <option value="BN" v-model="user.profile.country">Brunei Darussalam</option>
              <option value="BG" v-model="user.profile.country">Bulgarien</option>
              <option value="BF" v-model="user.profile.country">Burkina Faso</option>
              <option value="BI" v-model="user.profile.country">Burundi</option>
              <option value="KY" v-model="user.profile.country">Cayman Islands</option>
              <option value="CL" v-model="user.profile.country">Chile</option>
              <option value="CN" v-model="user.profile.country">China</option>
              <option value="CK" v-model="user.profile.country">Cookinseln</option>
              <option value="CR" v-model="user.profile.country">Costa Rica</option>
              <option value="CW" v-model="user.profile.country">Curaçao</option>
              <option value="DK" v-model="user.profile.country">Dänemark</option>
              <option value="DE" v-model="user.profile.country">Deutschland</option>
              <option value="DM" v-model="user.profile.country">Dominica</option>
              <option value="DO" v-model="user.profile.country">Domenikanische Republik</option>
              <option value="EC" v-model="user.profile.country">Ecuador</option>
              <option value="CI" v-model="user.profile.country">Elfenbeinküste</option>
              <option value="SV" v-model="user.profile.country">El Salvador</option>
              <option value="GB" v-model="user.profile.country">England</option>
              <option value="ER" v-model="user.profile.country">Eritrea</option>
              <option value="EE" v-model="user.profile.country">Estland</option>
              <option value="ET" v-model="user.profile.country">Ethiopien</option>
              <option value="FK" v-model="user.profile.country">Falklandinseln (Malvinien)</option>
              <option value="FO" v-model="user.profile.country">Färörer Inseln</option>
              <option value="FJ" v-model="user.profile.country">Fiji</option>
              <option value="FI" v-model="user.profile.country">Finnland</option>
              <option value="FR" v-model="user.profile.country">Frankreich</option>
              <option value="GF" v-model="user.profile.country">Französisch-Guyana</option>
              <option value="PF" v-model="user.profile.country">Französisch-Polynesien</option>
              <option value="GA" v-model="user.profile.country">Gabun</option>
              <option value="GM" v-model="user.profile.country">Gambia</option>
              <option value="GE" v-model="user.profile.country">Georgien</option>
              <option value="GH" v-model="user.profile.country">Ghana</option>
              <option value="GI" v-model="user.profile.country">Gibraltar</option>
              <option value="GD" v-model="user.profile.country">Grenada</option>
              <option value="GR" v-model="user.profile.country">Griechenland</option>
              <option value="GL" v-model="user.profile.country">Grönland</option>
              <option value="GP" v-model="user.profile.country">Guadeloupe</option>
              <option value="GU" v-model="user.profile.country">Guam</option>
              <option value="GT" v-model="user.profile.country">Guatemala</option>
              <option value="GG" v-model="user.profile.country">Guernsey</option>
              <option value="GN" v-model="user.profile.country">Guinea</option>
              <option value="GW" v-model="user.profile.country">Guinea-Bissau</option>
              <option value="GY" v-model="user.profile.country">Guyana</option>
              <option value="HT" v-model="user.profile.country">Haiti</option>
              <option value="HM" v-model="user.profile.country">Heard Island and McDonald Islands</option>
              <option value="HN" v-model="user.profile.country">Honduras</option>
              <option value="HK" v-model="user.profile.country">Hong Kong</option>
              <option value="IN" v-model="user.profile.country">Indien</option>
              <option value="ID" v-model="user.profile.country">Indonesien</option>
              <option value="IR" v-model="user.profile.country">Iran</option>
              <option value="IQ" v-model="user.profile.country">Irak</option>
              <option value="IE" v-model="user.profile.country">Irland</option>
              <option value="IM" v-model="user.profile.country">Isle of Man</option>
              <option value="IS" v-model="user.profile.country">Island</option>
              <option value="IL" v-model="user.profile.country">Israel</option>
              <option value="IT" v-model="user.profile.country">Italien</option>
              <option value="JM" v-model="user.profile.country">Jamaika</option>
              <option value="JP" v-model="user.profile.country">Japan</option>
              <option value="YE" v-model="user.profile.country">Jemen</option>
              <option value="JE" v-model="user.profile.country">Jersey</option>
              <option value="JO" v-model="user.profile.country">Jordan</option>
              <option value="VG" v-model="user.profile.country">Jungferninseln</option>
              <option value="KH" v-model="user.profile.country">Kambodscha</option>
              <option value="CM" v-model="user.profile.country">Kamerun</option>
              <option value="CA" v-model="user.profile.country">Kanada</option>
              <option value="CV" v-model="user.profile.country">Kap Verde</option>
              <option value="QA" v-model="user.profile.country">Katar</option>
              <option value="KZ" v-model="user.profile.country">Kazakhstan</option>
              <option value="KE" v-model="user.profile.country">Kenya</option>
              <option value="KG" v-model="user.profile.country">Kirgistan</option>
              <option value="KI" v-model="user.profile.country">Kiribati</option>
              <option value="CO" v-model="user.profile.country">Kolombien</option>
              <option value="KM" v-model="user.profile.country">Komoren</option>
              <option value="CG" v-model="user.profile.country">Kongo</option>
              <option value="HR" v-model="user.profile.country">Kroatien</option>
              <option value="CU" v-model="user.profile.country">Kuba</option>
              <option value="KW" v-model="user.profile.country">Kuwait</option>
              <option value="LA" v-model="user.profile.country">Laos</option>
              <option value="LS" v-model="user.profile.country">Lesotho</option>
              <option value="LV" v-model="user.profile.country">Lettland</option>
              <option value="LB" v-model="user.profile.country">Libanon</option>
              <option value="LR" v-model="user.profile.country">Liberien</option>
              <option value="LY" v-model="user.profile.country">Libien</option>
              <option value="LI" v-model="user.profile.country">Liechtenstein</option>
              <option value="LT" v-model="user.profile.country">Litauen</option>
              <option value="LU" v-model="user.profile.country">Luxemburg</option>
              <option value="MO" v-model="user.profile.country">Macao</option>
              <option value="MG" v-model="user.profile.country">Madagaskar</option>
              <option value="MW" v-model="user.profile.country">Malawi</option>
              <option value="MY" v-model="user.profile.country">Malaysien</option>
              <option value="MV" v-model="user.profile.country">Malediven</option>
              <option value="ML" v-model="user.profile.country">Mali</option>
              <option value="MT" v-model="user.profile.country">Malta</option>
              <option value="MH" v-model="user.profile.country">Marshall Islands</option>
              <option value="MQ" v-model="user.profile.country">Martinique</option>
              <option value="MR" v-model="user.profile.country">Mauritanien</option>
              <option value="MU" v-model="user.profile.country">Mauritius</option>
              <option value="YT" v-model="user.profile.country">Mayotte</option>
              <option value="MK" v-model="user.profile.country">Mazedonien</option>
              <option value="MX" v-model="user.profile.country">Mexiko</option>
              <option value="FM" v-model="user.profile.country">Mikronesien</option>
              <option value="MD" v-model="user.profile.country">Moldovien</option>
              <option value="MC" v-model="user.profile.country">Monako</option>
              <option value="MN" v-model="user.profile.country">Mongolei</option>
              <option value="ME" v-model="user.profile.country">Montenegro</option>
              <option value="MS" v-model="user.profile.country">Montserrat</option>
              <option value="MA" v-model="user.profile.country">Morokko</option>
              <option value="MZ" v-model="user.profile.country">Mosambik</option>
              <option value="MM" v-model="user.profile.country">Myanmar</option>
              <option value="NA" v-model="user.profile.country">Namibien</option>
              <option value="NR" v-model="user.profile.country">Nauru</option>
              <option value="NP" v-model="user.profile.country">Nepal</option>
              <option value="NC" v-model="user.profile.country">Neukaledonien</option>
              <option value="NZ" v-model="user.profile.country">Neuseeland</option>
              <option value="NL" v-model="user.profile.country">Niederlande</option>
              <option value="NI" v-model="user.profile.country">Nicaragua</option>
              <option value="NE" v-model="user.profile.country">Niger</option>
              <option value="NG" v-model="user.profile.country">Nigerien</option>
              <option value="NU" v-model="user.profile.country">Niue</option>
              <option value="KP" v-model="user.profile.country">Nordkorea</option>
              <option value="NF" v-model="user.profile.country">Norfolk</option>
              <option value="NO" v-model="user.profile.country">Norwegen</option>
              <option value="OM" v-model="user.profile.country">Oman</option>
              <option value="AT" v-model="user.profile.country">Österreich</option>
              <option value="TL" v-model="user.profile.country">Osttimor</option>
              <option value="PK" v-model="user.profile.country">Pakistan</option>
              <option value="PW" v-model="user.profile.country">Palau</option>
              <option value="PS" v-model="user.profile.country">Palästinensische Autonomiegebiete</option>
              <option value="PA" v-model="user.profile.country">Panama</option>
              <option value="PG" v-model="user.profile.country">Papua-Neuguinea</option>
              <option value="PY" v-model="user.profile.country">Paraguay</option>
              <option value="PE" v-model="user.profile.country">Peru</option>
              <option value="PH" v-model="user.profile.country">Philippinen</option>
              <option value="PN" v-model="user.profile.country">Pitcairninseln</option>
              <option value="PL" v-model="user.profile.country">Polen</option>
              <option value="PT" v-model="user.profile.country">Portugal</option>
              <option value="PR" v-model="user.profile.country">Puerto Rico</option>
              <option value="RE" v-model="user.profile.country">Réunion</option>
              <option value="CD" v-model="user.profile.country">Republik Kongo</option>
              <option value="RO" v-model="user.profile.country">Romänien</option>
              <option value="RW" v-model="user.profile.country">Ruanda</option>
              <option value="RU" v-model="user.profile.country">Russland</option>
              <option value="BL" v-model="user.profile.country">Saint Barthélemy</option>
              <option value="SH" v-model="user.profile.country">Sankt Helena</option>
              <option value="KN" v-model="user.profile.country">Sankt Kitts und Nevis</option>
              <option value="LC" v-model="user.profile.country">Sankt Lucia</option>
              <option value="MF" v-model="user.profile.country">Sankt Martin</option>
              <option value="PM" v-model="user.profile.country">Sankt Pierre und Miquelon</option>
              <option value="VC" v-model="user.profile.country">Sankt Vincent und die Grenadinen</option>
              <option value="ZM" v-model="user.profile.country">Sambia</option>
              <option value="WS" v-model="user.profile.country">Samoa</option>
              <option value="SM" v-model="user.profile.country">San Marino</option>
              <option value="ST" v-model="user.profile.country">Sao Tome und Principe</option>
              <option value="SA" v-model="user.profile.country">Saudi Arabien</option>
              <option value="SE" v-model="user.profile.country">Schweden</option>
              <option value="CH" v-model="user.profile.country">Schweiz</option>
              <option value="SN" v-model="user.profile.country">Senegal</option>
              <option value="RS" v-model="user.profile.country">Serbien</option>
              <option value="SC" v-model="user.profile.country">Seychellen</option>
              <option value="SL" v-model="user.profile.country">Sierra Leone</option>
              <option value="ZW" v-model="user.profile.country">Simbabwe</option>
              <option value="SG" v-model="user.profile.country">Singapur</option>
              <option value="SX" v-model="user.profile.country">Sint Maarten</option>
              <option value="SK" v-model="user.profile.country">Slovakei</option>
              <option value="SI" v-model="user.profile.country">Slovenien</option>
              <option value="SB" v-model="user.profile.country">Solomonen</option>
              <option value="SO" v-model="user.profile.country">Somalia</option>
              <option value="ES" v-model="user.profile.country">Spanien</option>
              <option value="LK" v-model="user.profile.country">Sri Lanka</option>
              <option value="SD" v-model="user.profile.country">Sudan</option>
              <option value="ZA" v-model="user.profile.country">Süd Afrika</option>
              <option value="KR" v-model="user.profile.country">Südkorea</option>
              <option value="SS" v-model="user.profile.country">Süd Sudan</option>
              <option value="SR" v-model="user.profile.country">Suriname</option>
              <option value="SJ" v-model="user.profile.country">Svalbard und Jan Mayen</option>
              <option value="SZ" v-model="user.profile.country">Swaziland</option>
              <option value="SY" v-model="user.profile.country">Syrien</option>
              <option value="TJ" v-model="user.profile.country">Tadschikistan</option>
              <option value="TW" v-model="user.profile.country">Taiwan</option>
              <option value="TZ" v-model="user.profile.country">Tansania</option>
              <option value="TH" v-model="user.profile.country">Thailand</option>
              <option value="TG" v-model="user.profile.country">Togo</option>
              <option value="TK" v-model="user.profile.country">Tokelau</option>
              <option value="TO" v-model="user.profile.country">Tonga</option>
              <option value="TT" v-model="user.profile.country">Trinidad und Tobago</option>
              <option value="TD" v-model="user.profile.country">Tschad</option>
              <option value="CZ" v-model="user.profile.country">Tschechien</option>
              <option value="DJ" v-model="user.profile.country">Tschibouti</option>
              <option value="TN" v-model="user.profile.country">Tunisien</option>
              <option value="TM" v-model="user.profile.country">Turkmenistan</option>
              <option value="TC" v-model="user.profile.country">Turks und Caicosinseln</option>
              <option value="TR" v-model="user.profile.country">Türkei</option>
              <option value="TV" v-model="user.profile.country">Tuvalu</option>
              <option value="UG" v-model="user.profile.country">Uganda</option>
              <option value="UA" v-model="user.profile.country">Ukraine</option>
              <option value="HU" v-model="user.profile.country">Ungarn</option>
              <option value="UY" v-model="user.profile.country">Uruguai</option>
              <option value="UZ" v-model="user.profile.country">Usbekistan</option>
              <option value="VU" v-model="user.profile.country">Vanuatu</option>
              <option value="VA" v-model="user.profile.country">Vatikan</option>
              <option value="VE" v-model="user.profile.country">Venezuela</option>
              <option value="VN" v-model="user.profile.country">Vietnam</option>
              <option value="WF" v-model="user.profile.country">Wallis und Futuna</option>
              <option value="CX" v-model="user.profile.country">Weihnachtsinsel</option>
              <option value="EH" v-model="user.profile.country">Westahara</option>
              <option value="CF" v-model="user.profile.country">Zentralafrika</option>
              <option value="CY" v-model="user.profile.country">Zypern</option>
            </select>
          </div>
        </div>
          <div class="form-item">
            <div class="info-block">
              <span class="label">Firma</span>
              <input class="input-text" type="text" v-model="user.profile.company" name="" id=""/>
            </div>
          </div>
      </div>
      <div>
        <img v-if="imageData != null" class="preview" :src="imageData" style="height: 300px; width: 300px; margin: 10px">
      </div>
    </div>
  </div>
</template>

<script>
import Checkbox from "~/components/Checkbox.vue";

export default {
  middleware: 'authenticated',
  components: {
    Checkbox
  },
  data () {
    return {
      loading: false,
      dsBool: false,
      picture: '',
      imageEvent: null,
      imageData: null
    }
  },
  created() {
  },
  methods: {
    updateUser(event) {
      this.loading = true;
      this.$store.dispatch('updateUser', Object.assign({}, this.user.profile)).then(() => {
        this.loading = false;
        this.$notify({
          title: 'Yay!',
          text: 'Änderungen gespeichert.'
        });
      }).catch((e) => {
        this.loading = false;
        this.$notify({
          title: 'Error',
          type: 'error',
          text: 'Ein Fehler ist aufgetreten.'
        });
      });
    },
    getPicture(e) {
      console.log(e.target.value);
      this.picture = this.$refs.picture.files[0];
      var input = e.target;
      if (input.files && input.files[0]) {
        var reader = new FileReader();
        reader.onload = (event) => {
          this.imageData = event.target.result;
        }
        reader.readAsDataURL(input.files[0]);
      }
      console.log(this.picture);
      this.user.picture = this.picture;
      this.user.input = e.target;

    }
  },
  computed: {
    user() {
      return this.$store.state.user;
    },
    window(){
      return window.location.origin
    },
  }
}
</script>

<style lang="scss">
  @import '@/assets/scss/styles.scss';
  .info-block {
    display: flex;
    justify-content: space-between;
    @include media-breakpoint-up(sm) {
      width: 80%;
    }
  }
  .checkbox-link {
    padding-left: 5px;
    padding-right: 5px;
  }
  .label {
    padding-right: 10px;
  }
  .form-item {
    margin: 15px;
  }
  .wizard-checkbox {
    margin-top: 40px;
  }
  input[type="file"] {
    display: none;
  }
  .picture-file-upload {
    cursor: pointer;
    background-color: #ff6f00;
    color: #FFF;
    min-width: 40%;
    border: 1px solid #ff8c33;
    padding: 7px 12px 8px;
    line-height: 1;
    outline: none;
  }

  .select{
    width: 51%;
  }
  .sectionFlex  {
    @include media-breakpoint-up(sm) {
      display: flex;
    }
  }
  .zipError {
    width: 49%;
  }
</style>
