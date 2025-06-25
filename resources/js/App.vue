<template>
    <div class="min-h-screen p-6 bg-gray-50">
        <h2 class="mb-6 text-3xl font-bold text-center text-gray-800">PDF Preview</h2>

        <Form ref="userForm" class="max-w-xl mx-auto mb-6" />

        <div class="mb-8 text-center">
            <button @click="generatePDF"
                class="px-6 py-3 font-bold text-white transition duration-300 ease-in-out bg-blue-600 rounded-lg shadow-lg hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-500 focus:ring-opacity-75">
                Generate and View PDF
            </button>
        </div>

        <div class="flex justify-center mt-4">
            <iframe v-if="pdfUrl" :src="pdfUrl" width="70%" height="800px"
                class="border border-gray-300 rounded-lg shadow-xl"></iframe>
        </div>
    </div>
</template>

<script setup>
import { ref } from 'vue';
import jsPDF from 'jspdf';
import Form from './Content.vue'; // Adjust path as necessary

const pdfUrl = ref(null);
const userForm = ref(null); // Ref to access the Form component

function generatePDF() {
    const doc = new jsPDF();
    const pageWidth = doc.internal.pageSize.getWidth();

    // Set Thai font (make sure it's added via custom font setup)
    doc.setFont('Sarabun-Regular', 'normal');
    doc.setFontSize(16);

    // Centered title
    const title = 'สวัสดีครับ คุณเป็นอย่างไรบ้าง';
    const titleWidth = doc.getTextWidth(title);
    const titleX = (pageWidth - titleWidth) / 2;
    doc.text(title, titleX, 30);


    // --- Add Document Number to the top right ---
    const documentNumber = 'DOC-2025-001'; // Define your document number here
    doc.setFontSize(10); // Smaller font size for the document number
    doc.setFont('Sarabun-Regular', 'normal'); // Ensure font is set for the number

    // Calculate X position for right alignment: pageWidth - textWidth - rightMargin
    const docNumWidth = doc.getTextWidth(documentNumber);
    const rightMargin = 20; // Adjust as needed
    const docNumX = pageWidth - docNumWidth - rightMargin;
    const docNumY = 15; // Y position in the header area



    doc.text(documentNumber, docNumX, docNumY);
    // --- End Document Number addition ---

    doc.setFontSize(12);
    doc.text('นี่คือรายงานประจำเดือนของคุณ:', 20, 45);
    doc.text('วันที่พิมพ์: ' + new Date().toLocaleString('th-TH'), 20, 55);

    // --- Add data from Form.vue ---
    let currentY = 70; // Starting Y position for form data

    if (userForm.value && userForm.value.formData) {
        const formData = userForm.value.formData;

        doc.setFontSize(14);
        doc.setFont('Sarabun-Bold', 'normal');
        doc.text('ข้อมูลผู้ใช้:', 20, currentY);
        currentY += 10;

        doc.setFontSize(12);
        doc.setFont('Sarabun-Regular', 'normal');
        doc.text(`ชื่อ: ${formData.name || 'ไม่ได้ระบุ'}`, 20, currentY);
        currentY += 10;
        doc.text(`อีเมล: ${formData.email || 'ไม่ได้ระบุ'}`, 20, currentY);
        currentY += 10;
        doc.text(`ข้อความ: ${formData.message || 'ไม่ได้ระบุ'}`, 20, currentY);
        currentY += 20; // Add some extra space before the table
    }

    // Table setup
    const startX = 20;
    const rowHeight = 10;
    const colWidths = [60, 40, 50];

    const headers = ['สินค้า', 'จำนวน', 'ราคา'];
    const data = [
        ['ข้าวสาร', '2', '100 บาท'],
        ['น้ำตาล', '1', '50 บาท'],
    ];

    // Draw table header
    headers.forEach((header, i) => {
        doc.setFontSize(14);
        doc.setFont('Sarabun-Bold', 'normal');
        doc.rect(startX + colWidths.slice(0, i).reduce((a, b) => a + b, 0), currentY, colWidths[i], rowHeight);
        doc.text(header, startX + colWidths.slice(0, i).reduce((a, b) => a + b, 0) + 2, currentY + 7);
    });

    // Update currentY after header
    currentY += rowHeight;

    // Draw table rows
    data.forEach((row, rowIndex) => {
        doc.setFontSize(12);
        doc.setFont('Sarabun-Regular', 'normal');
        const currentRowY = currentY + rowHeight * rowIndex;
        row.forEach((cell, colIndex) => {
            const cellX = startX + colWidths.slice(0, colIndex).reduce((a, b) => a + b, 0);
            doc.rect(cellX, currentRowY, colWidths[colIndex], rowHeight);
            doc.text(cell, cellX + 2, currentRowY + 7);
        });
    });

    // Generate PDF blob
    const blob = doc.output('blob');
    pdfUrl.value = URL.createObjectURL(blob);
}
</script>

<style scoped>
/* No styles needed here if you're using Tailwind CSS consistently */
</style>
