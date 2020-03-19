<template>
  <v-container id="capture">
    <v-app id="inspire">
      <v-form ref="form">
        <v-text-field v-model="customerInfo.name" placeholder="Mme/M" required></v-text-field>
        <v-text-field
          v-model="customerInfo.birthday"
          placeholder="Né(e) le : (JJ/MM/AAAA)"
          required
        ></v-text-field>
        <v-text-field v-model="customerInfo.adress" placeholder="Adresse complète :" required></v-text-field>
        <v-radio-group v-model="customerInfo.option" column>
          <v-radio
            style="color: darkblue;"
            label="Déplacements entre le domicile et le lieu d’exercice de l’activité professionnelle, lorsqu’ils sont indispensables à l’exercice d’activités ne pouvant être organisées sous forme de télétravail (sur justificatif permanent) ou déplacements professionnels ne pouvant être différés"
            value="work"
          ></v-radio>
          <v-radio
            style="color: darkblue;"
            label="Déplacements pour effectuer des achats de première nécessité dans des établissements autorisés (liste sur gouvernement.fr)"
            value="mall"
          ></v-radio>
          <v-radio style="color: darkblue;" label="Déplacements pour motif de santé" value="healt"></v-radio>
          <v-radio
            style="color: darkblue;"
            label="Déplacements pour motif familial impérieux, pour l’assistance aux personnes vulnérables ou la garde d’enfants"
            value="family"
          ></v-radio>
          <v-radio
            style="color: darkblue;"
            label="Déplacements brefs, à proximité du domicile, liés à l’activité physique individuelle des personnes, à l’exclusion de toute pratique sportive collective, et aux besoins des animaux de compagnie."
            value="sport"
          ></v-radio>
        </v-radio-group>
        <div style="display:block;margin-bottom: 50px;">
          <p class="date">Le {{ date }}</p>
          <v-text-field
            v-model="customerInfo.signed_at"
            placeholder="Fait à:"
            required
            class="signed_at"
          ></v-text-field>
          <p class="signature">Signature:</p>
          <div class="digit-sign">
            <VueSignaturePad
              width="500px"
              height="150px"
              ref="signaturePad"
              style="border: 1px solid;margin:0 auto;"
            />
          </div>
        </div>
        <div class="groupe-btn">
          <v-btn color="success" class="mr-4" @click="undo()">Éffacer ma signature</v-btn>
          <v-btn color="success" class="mr-4" @click="generatePdf()">Générer mon attestation.</v-btn>
        </div>
      </v-form>
    </v-app>
  </v-container>
</template>

<script>
import jsPDF from "jspdf";
import html2canvas from "html2canvas";
import VueSignaturePad from "vue-signature-pad";
export default {
  name: "Form",
  components: {
    VueSignaturePad
  },
  data: () => ({
    customerInfo: {
      name: "",
      birthday: "",
      address: "",
      signed_at: "",
      option: ""
    },
    date: new Date().toLocaleString().split(",")[0]
  }),
  methods: {
    generatePdf() {
      const data = document.getElementById("main");
      data.style.width = "1200px";
      data.style.height = "1200px";
      html2canvas(data).then(canvas => {
        const imgWidth = 208;
        const imgHeight = (canvas.height * imgWidth) / canvas.width;
        console.log(imgHeight);
        const contentDataURL = canvas.toDataURL("image/png");
        let pdf = new jsPDF("p", "mm", "a4", 1);
        const position = 0;
        pdf.addImage(contentDataURL, "PNG", 0, position, imgWidth, imgHeight);
        this.url = canvas.toDataURL();
        pdf.save(
          "ATTESTATION_DE_DÉPLACEMENT_DÉROGATOIRE-" + this.date + ".pdf"
        );
        data.style.width = window.innerWidth + "px";
      });
    },
    undo() {
      this.$refs.signaturePad.undoSignature();
    }
  }
};
</script>
<style scoped>
.mr-4 {
  width: 50%;
  margin-bottom: 20px;
}
.date {
  display: inline-block;
  width: 30%;
}
.signed_at {
  display: inline-block;
  width: 30%;
  margin-right: 150px;
}
.signature {
  display: inline-block;
  width: 6%;
}
.digit-sign {
  width: 100%;
  margin: 0 auto;
  margin-bottom: 100px;
}
.groupe-btn {
  width: 100%;
  margin: 0 auto;
  text-align: center;
}
</style>
