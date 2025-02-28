# excel-generator
<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Generar Reporte de Inventario</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/xlsx/0.17.5/xlsx.full.min.js"></script>
    <style>
        body { font-family: Arial, sans-serif; text-align: center; }
        .btn { background-color: #007bff; color: white; padding: 10px 20px; border: none; cursor: pointer; margin-top: 10px; }
        #enlace { margin-top: 20px; }
    </style>
</head>
<body>
    <h2>Generar Reporte de Inventario</h2>
    <button class="btn" onclick="generarExcel()">Generar Excel</button>

    <div id="enlace"></div>

    <script>
        function generarNombreAleatorio() {
            const caracteres = 'ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789';
            let nombre = 'Inventario_Reporte_';
            for (let i = 0; i < 8; i++) {
                nombre += caracteres.charAt(Math.floor(Math.random() * caracteres.length));
            }
            return nombre + '.xlsx';
        }

        function generarExcel() {
            // Simular datos de inventario
            let inventario = [
                { Producto: "Producto A", "Cantidad en Sistema": 10, "Cantidad Física": 8, "Diferencia": 2 },
                { Producto: "Producto B", "Cantidad en Sistema": 5, "Cantidad Física": 5, "Diferencia": 0 },
                { Producto: "Producto C", "Cantidad en Sistema": 0, "Cantidad Física": 1, "Diferencia": -1 }
            ];

            // Convertir a formato Excel
            let ws = XLSX.utils.json_to_sheet(inventario);
            let wb = XLSX.utils.book_new();
            XLSX.utils.book_append_sheet(wb, ws, "Reporte Inventario");

            // Generar archivo en memoria
            let wbout = XLSX.write(wb, { bookType: 'xlsx', type: 'array' });
            let blob = new Blob([wbout], { type: "application/vnd.openxmlformats-officedocument.spreadsheetml.sheet" });

            // Generar un nombre aleatorio para el archivo
            let nombreArchivo = generarNombreAleatorio();

            // Crear un objeto URL para el blob
            let url = URL.createObjectURL(blob);

            // Crear un enlace para descargar el archivo
            let enlace = document.createElement("a");
            enlace.href = url;
            enlace.download = nombreArchivo; // Sugerir nombre de archivo
            enlace.textContent = "Haz clic aquí para descargar el archivo Excel: " + nombreArchivo;
            enlace.style.display = "block"; // Mostrar el enlace como bloque

            // Limpiar el contenedor de enlaces y agregar el nuevo enlace
            const contenedorEnlace = document.getElementById("enlace");
            contenedorEnlace.innerHTML = ''; // Limpiar contenido anterior
            contenedorEnlace.appendChild(enlace);

            // Limpiar el objeto URL después de un tiempo
            setTimeout(() => {
                URL.revokeObjectURL(url);
            }, 1000);
        }
    </script>
</body>
</html>
