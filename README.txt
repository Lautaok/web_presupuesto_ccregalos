<!DOCTYPE html>
<html lang="es">

<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Presupuestos Burgos Vidrios</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <style>
        body {
            font-family: Arial, sans-serif;
            margin: 0 auto;
            padding: 18px;
            max-width: 1000px;
            background-color: #f4f4f4;
            color: #333;
        }

        .presupuesto-container {
            background-color: #fff;
            padding: 15px;
            border: 1px solid #ccc;
            box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
        }

        header {
            display: flex;
            justify-content: space-between;
            align-items: flex-start;
            border-bottom: 2px solid #000;
            padding-bottom: 10px;
            margin-bottom: 20px;
            position: relative;
        }

        .datos-empresa {
            text-align: left;
            flex: 1;
        }

        .datos-empresa img {
            max-width: 140px;
            margin-bottom: 10px;
        }

        .datos-empresa p {
            margin: 4px 0;
        }

        .datos-presupuesto {
            text-align: right;
            border: 1px solid #000;
            padding: 5px;
            flex-shrink: 0;
            width: 350px;
            text-align: center;
        }

        .datos-presupuesto p {
            margin: 4px 0;
        }

        .datos-presupuesto h2 {
            margin: 0;
            font-size: 1.4em;
            text-transform: uppercase;
            text-align: center;
        }

        .box-x {
            position: absolute;
            top: 10px;
            left: 50%;
            transform: translateX(-50%);
            border: 2px solid #000;
            padding: 10px 20px;
            font-size: 2em;
            font-weight: bold;
            background-color: #fff;
            z-index: 10;
        }

        #cliente,
        #items,
        #totales,
        footer {
            margin-bottom: 20px;
        }

        .cliente-info {
            display: grid;
            grid-template-columns: 1fr 1fr;
            gap: 10px;
        }

        .cliente-info div {
            display: flex;
            flex-direction: column;
        }

        label {
            font-weight: bold;
            margin-bottom: 5px;
            font-size: 0.9em;
        }

        input[type="text"],
        input[type="number"],
        input[type="date"],
        textarea,
        select {
            padding: 5px;
            border: 1px solid #ccc;
            border-radius: 4px;
            width: 100%;
            box-sizing: border-box;
            font-size: 0.9em;
            font-family: Arial, sans-serif;
        }

        input[type="date"] {
            width: auto;
        }

        table {
            width: 100%;
            border-collapse: collapse;
            margin-top: 20px;
            font-size: small;
        }

        th,
        td {
            border: 1px solid #000;
            padding: 5px;
            text-align: center;
            font-size: 1.1rem;
        }

        th {
            background-color: #e0e0e0;
            font-weight: bold;
            text-transform: uppercase;
            font-size: 0.8em;
        }

        td input {
            text-align: center;
            border: none;
            background-color: transparent;
            font-size: 0.9em;
            padding: 0;
            width: 100%;
            /* Ajuste de ancho para prevenir que el texto se corte */
        }

        td .codigo {
            text-align: center;
        }

        td .cantidad {
            text-align: center;
        }

        td.descripcion input {
            text-align: left;
        }

        .total-general {
            text-align: right;
            font-size: 1.2em;
            font-weight: bold;
            padding-right: 10px;
        }

        .acciones {
            text-align: center;
            margin-top: 30px;
        }

        button {
            padding: 5px 10px;
            font-size: 1em;
            cursor: pointer;
            border: none;
            border-radius: 5px;
            margin: 0 10px;
            transition: background-color 0.3s ease;
        }

        #btn-pdf {
            background-color: #d9534f;
            color: white;
        }

        #btn-pdf:hover {
            background-color: #c9302c;
        }

        #btn-print {
            background-color: #5bc0de;
            color: white;
        }

        #btn-print:hover {
            background-color: #31b0d5;
        }

        .row-actions {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin-top: 10px;
        }

        #btn-add-row,
        #btn-remove-row {
            background-color: #5181e9;
            color: white;
        }

        #btn-add-row:hover,
        #btn-remove-row:hover {
            background-color: #4cae4c;
        }

        #btn-remove-row {
            background-color: #dc3545;
        }

        #btn-remove-row:hover {
            background-color: #c82333;
        }

        footer textarea {
            min-height: 60px;
        }

        /* Estilos para impresión */
        @media print {
             /* --- INICIO DE LA CORRECCIÓN --- */
            @page {
                margin: 0;
                size: auto;
            }
            /* --- FIN DE LA CORRECCIÓN --- */
            body {
                background-color: #fff;
                padding: 0;
                margin: 0;
            }

            .acciones,
            .row-actions,
            .no-print-pdf,
            .btn-remove-row {
                display: none !important;
            }

            .presupuesto-container {
                box-shadow: none;
                border: none;
                margin: 0;
                padding: 20px;
            }

            input,
            textarea,
            select {
                border: none !important;
                background-color: transparent !important;
                -webkit-print-color-adjust: exact;
                color-adjust: exact;
                -webkit-appearance: none;
                -moz-appearance: none;
                appearance: none;
            }

            input[type="date"]::-webkit-calendar-picker-indicator {
                display: none;
            }
        }

        /* Responsive Styles - Media Queries */
        /* Para pantallas pequeñas, como celulares */
        @media (max-width: 600px) {
            body {
                padding: 10px;
            }

            .presupuesto-container {
                padding: 15px;
            }

            header {
                flex-direction: column;
                align-items: center;
                text-align: center;
            }

            .datos-empresa {
                text-align: center;
                margin-bottom: 20px;
            }

            .datos-presupuesto {
                width: 100%;
                text-align: center;
            }

            .box-x {
                display: none;
            }

            .cliente-info {
                grid-template-columns: 1fr;
            }

            table {
                font-size: 0.7em;
                overflow-x: auto;
                display: block;
                white-space: nowrap;
            }

            td,
            th {
                padding: 5px;
                font-size: 0.8em;
            }

            .acciones,
            .row-actions {
                display: flex;
                flex-direction: column;
            }

            button {
                width: 100%;
                margin: 5px 0;
            }
        }
    </style>
</head>

<body>

    <div id="presupuesto" class="presupuesto-container">
        <header>
            <div class="datos-empresa">
                <img src="img/foto_portada1(1).png" alt="Logo de la Empresa" id="logo-empresa">
                <p><strong>Burgos Vidrios - Somos Fabricantes</strong></p>
                <p>San Pedro de Jujuy</p>
                <p>Teléfonos: 3888422636 / 3888317473</p>
                <p>IVA Responsable Inscripto</p>
            </div>
            <div class="box-x">X</div>
            <div class="datos-presupuesto">
                <h2>PRESUPUESTO</h2>
                <p>Documento no válido como Factura</p>
                <p><strong>Comprobante:</strong> <span id="nro-comprobante"></span></p>
                <p><strong>Fecha:</strong> <input type="date" id="fecha"></p>
                <p><strong>C.U.I.T.:</strong> 20-26850672-2</p>
                <p><strong>Inicio de Actividad:</strong> 15/09/2011</p>
            </div>
        </header>

        <main>
            <section id="cliente">
                <h3>Cliente</h3>
                <div class="cliente-info">
                    <div>
                        <label for="cliente-nombre">Cliente:</label>
                        <input type="text" id="cliente-nombre" placeholder="Nombre del Cliente">
                    </div>
                    <div>
                        <label for="cliente-cuit">C.U.I.T.:</label>
                        <input type="text" id="cliente-cuit" placeholder="C.U.I.T. del Cliente">
                    </div>
                    <div>
                        <label for="cliente-domicilio">Domicilio:</label>
                        <input type="text" id="cliente-domicilio" placeholder="Domicilio">
                    </div>
                    <div>
                        <label for="cliente-iva">Condición IVA:</label>
                        <select id="cliente-iva" name="cliente-iva">
                            <option value="Responsable Inscripto">Responsable Inscripto</option>
                            <option value="Monotributista">Monotributista</option>
                            <option value="Exento">Exento</option>
                            <option value="Consumidor Final">Consumidor Final</option>
                        </select>
                    </div>
                </div>
            </section>

            <section id="items">
                <table>
                    <thead>
                        <tr>
                            <th style="width: 15%;">Código</th>
                            <th style="width: 5%;">Cant.</th>
                            <th style="width: 45%;">Descripción</th>
                            <th style="width: 10%;">Precio Unit.</th>
                            <th style="width: 12%;">Total</th>
                        </tr>
                    </thead>
                    <tbody id="tabla-items">
                    </tbody>
                </table>
                <div class="row-actions no-print-pdf">
                    <button id="btn-add-row">Añadir Fila</button>
                    <button id="btn-remove-row">Eliminar Fila</button>
                </div>
            </section>

            <section id="totales" class="total-general">
                <p>Total: <span id="total-general">$0.00</span></p>
            </section>
        </main>

        <footer>
            <div>
                <label for="condicion-pago">Condición de Pago:</label>
                <input type="text" id="condicion-pago" placeholder="Ej: Contado, 30 días fecha factura, etc.">
            </div>
            <div style="margin-top: 10px;">
                <label for="observaciones">Observaciones:</label>
                <textarea id="observaciones" rows="3"></textarea>
            </div>
        </footer>
    </div>

    <div class="acciones no-print-pdf">
        <button id="btn-pdf">Guardar como PDF</button>
        <button id="btn-print">Imprimir</button>
    </div>
    <script>
        window.jsPDF = window.jspdf.jsPDF;

        document.addEventListener('DOMContentLoaded', () => {
            const tablaItems = document.getElementById('tabla-items');
            const btnAddRow = document.getElementById('btn-add-row');
            const btnRemoveRow = document.getElementById('btn-remove-row');
            const btnPdf = document.getElementById('btn-pdf');
            const btnPrint = document.getElementById('btn-print');

            const guardarDatos = () => {
                const datos = {
                    // Ya no guardamos datos de cliente para no recargarlos
                    items: []
                };

                tablaItems.querySelectorAll('.item-row').forEach(row => {
                    const item = {
                        codigo: row.querySelector('.codigo').value,
                        cantidad: row.querySelector('.cantidad').value,
                        descripcion: row.querySelector('.descripcion').value,
                        precio: row.querySelector('.precio').value,
                    };
                    datos.items.push(item);
                });
                // Guardamos un objeto casi vacío para mantener la estructura si se necesita en el futuro
                localStorage.setItem('presupuestoDatos', JSON.stringify(datos));
            };

            const cargarDatos = () => {
                // --- INICIO DE LA MODIFICACIÓN ---
                // Limpiamos los campos del cliente
                document.getElementById('cliente-nombre').value = '';
                document.getElementById('cliente-cuit').value = '';
                document.getElementById('cliente-domicilio').value = '';
                document.getElementById('condicion-pago').value = '';
                document.getElementById('observaciones').value = '';

                // Ponemos la fecha de hoy
                document.getElementById('fecha').valueAsDate = new Date();

                // Limpiamos la tabla y añadimos una fila nueva
                tablaItems.innerHTML = '';
                tablaItems.appendChild(crearFila());

                // Recalculamos para que todo muestre $0.00
                calcularTotales();
                // --- FIN DE LA MODIFICACIÓN ---
            };

            const calcularTotales = () => {
                let totalGeneral = 0;
                tablaItems.querySelectorAll('.item-row').forEach(row => {
                    const cantidad = parseFloat(row.querySelector('.cantidad').value) || 0;
                    const precio = parseFloat(row.querySelector('.precio').value) || 0;
                    const totalFila = cantidad * precio;
                    row.querySelector('.total-fila').textContent = `$${totalFila.toFixed(2)}`;
                    totalGeneral += totalFila;
                });
                document.getElementById('total-general').textContent = `$${totalGeneral.toFixed(2)}`;
                // Ya no guardamos datos automáticamente al calcular para no sobreescribir
            };

            const crearFila = () => {
                const row = document.createElement('tr');
                row.classList.add('item-row');
                row.innerHTML = `
                <td><input type="text" class="codigo" placeholder="Código"></td>
                <td><input type="number" class="cantidad" value="1" min="0"></td>
                <td class="descripcion"><input type="text" class="descripcion" placeholder="Descripción del producto o servicio"></td>
                <td><input type="number" class="precio" value="0.00" step="0.01" min="0"></td>
                <td class="total-fila">$0.00</td>
            `;
                return row;
            };

            // Resto del código JavaScript...
            btnAddRow.addEventListener('click', () => {
                tablaItems.appendChild(crearFila());
            });

            btnRemoveRow.addEventListener('click', () => {
                const filas = tablaItems.querySelectorAll('.item-row');
                if (filas.length > 1) {
                    filas[filas.length - 1].remove();
                    calcularTotales();
                } else {
                    alert('No se puede eliminar la última fila.');
                }
            });

            tablaItems.addEventListener('input', calcularTotales);

            const generarNroComprobante = () => {
                let ultimoNro = localStorage.getItem('ultimoNroComprobante');
                let nuevoNro = ultimoNro ? parseInt(ultimoNro) + 1 : 1;
                localStorage.setItem('ultimoNroComprobante', nuevoNro);
                document.getElementById('nro-comprobante').textContent = `0001-${nuevoNro.toString().padStart(8, '0')}`;
            };

            btnPdf.addEventListener('click', () => {
                const elementoPresupuesto = document.getElementById('presupuesto');
                document.querySelectorAll('.no-print-pdf').forEach(el => el.style.display = 'none');
                elementoPresupuesto.style.width = '1000px';
                elementoPresupuesto.style.maxWidth = '1000px';

                html2canvas(elementoPresupuesto, {
                    scale: 2,
                    useCORS: true,
                    windowWidth: 1000
                }).then(canvas => {
                    const imgData = canvas.toDataURL('image/png');
                    const pdf = new jsPDF('p', 'mm', 'a4');
                    const pdfWidth = pdf.internal.pageSize.getWidth();
                    const pdfHeight = (canvas.height * pdfWidth) / canvas.width;

                    pdf.addImage(imgData, 'PNG', 0, 0, pdfWidth, pdfHeight);
                    pdf.save(`presupuesto-${document.getElementById('nro-comprobante').textContent}.pdf`);

                    document.querySelectorAll('.no-print-pdf').forEach(el => el.style.display = '');
                    elementoPresupuesto.style.width = '';
                    elementoPresupuesto.style.maxWidth = '';
                });
            });

            btnPrint.addEventListener('click', () => { window.print(); });

            generarNroComprobante();
            cargarDatos();
        });
    </script>

</body>

</html>