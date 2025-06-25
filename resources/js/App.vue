<!-- PdfViewer.vue -->
<template>
  <div>
    <h2>PDF Preview</h2>
    <button @click="generatePDF">Generate and View PDF</button>
    <iframe v-if="pdfUrl" :src="pdfUrl" width="100%" height="600px"></iframe>
  </div>
</template>

<script setup>
import { ref } from 'vue'
import jsPDF from 'jspdf'


const pdfUrl = ref(null)

function generatePDF() {
  const doc = new jsPDF()

  doc.setFont('Sarabun-Regular','normal')       // ✅ Use Thai font
  doc.setFontSize(16)
  doc.text('สวัสดีครับ คุณเป็นอย่างไรบ้าง', 20, 30)

  doc.setFontSize(12)
  doc.text('วันที่พิมพ์: ' + new Date().toLocaleString('th-TH'), 20, 50)

  const blob = doc.output('blob')
  pdfUrl.value = URL.createObjectURL(blob)
}
</script>
