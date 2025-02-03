#include <iostream>
#include <fstream> // Biblioteca para manejar archivos
#include <string>  // Biblioteca para manejar cadenas de texto
#include <algorithm> // Para transformar texto a minusculas

using namespace std;

// Estructura para almacenar los datos de cada dia
struct Dia {
    int numero;
    int tempMinima;
    int tempMaxima;
};

// Funcion para comparar dias por temperatura minima (para ordenar)
bool compararPorTempMinima(const Dia& a, const Dia& b) {
    return a.tempMinima < b.tempMinima;
}

void procesarArchivo(string nombreArchivo, string palabraBuscar, string palabraReemplazar) {
    // Abrir el archivo en modo lectura
    ifstream archivo(nombreArchivo.c_str());
    
    // Verificar si el archivo se abrio correctamente
    if (!archivo) {
        cout << "No se pudo abrir el archivo: " << nombreArchivo << endl;
        return;
    }
    
    string linea;
    int contador = 0; // Contador de ocurrencias de la palabra buscada
    string contenidoArchivo; // Almacena todo el contenido del archivo
    
    // Leer el archivo linea por linea
    while (getline(archivo, linea)) {
        size_t pos = 0;
        // Buscar la palabra en la linea actual
        while ((pos = linea.find(palabraBuscar, pos)) != string::npos) {
            contador++; // Incrementar el contador de ocurrencias
            pos += palabraBuscar.length(); // Avanzar la posicion para seguir buscando
        }
        contenidoArchivo += linea + "\n"; // Almacenar la linea en el contenido total
    }
    archivo.close(); // Cerrar el archivo
    
    // Mostrar informacion sobre el archivo procesado
    cout << "\nArchivo: " << nombreArchivo << endl;
    if (contador > 0) {
        cout << "La palabra '" << palabraBuscar << "' se encontro " << contador << " veces en el archivo." << endl;
    } else {
        cout << "La palabra '" << palabraBuscar << "' no se encontro en el archivo." << endl;
    }
    
    // Mostrar el contenido con reemplazo solo en consola
    cout << "Contenido modificado solo en consola:" << endl;
    size_t pos = 0;
    while ((pos = contenidoArchivo.find(palabraBuscar, pos)) != string::npos) {
        contenidoArchivo.replace(pos, palabraBuscar.length(), palabraReemplazar); // Reemplazar la palabra
        pos += palabraReemplazar.length(); // Avanzar la posicion para seguir buscando
    }
    cout << contenidoArchivo; // Mostrar el contenido modificado

    // Mostrar mensaje adicional solo para el archivo de clima
    if (nombreArchivo == "clima_riobamba_enero_2025.txt") {
        cout << "\n\n--- Contenido adicional sobre el clima ---\n";
        cout << "El clima en Riobamba sigue siendo variable en enero del 2025.\n";
        cout << "Se esperan lluvias ocasionales y temperaturas entre 7C y 16C.\n";
        cout << "Es recomendable llevar ropa abrigada y paraguas.\n";

        // Crear un array estatico para almacenar los datos de los dias
        Dia dias[31] = {
            {19, 2, 12}, {26, 2, 16}, {5, 2, 14}, {6, 2, 13}, {7, 2, 13},
            {17, 2, 16}, {23, 2, 13}, {3, 2, 14}, {16, 2, 14}, {11, 2, 14},
            {20, 2, 13}, {21, 2, 13}, {12, 3, 14}, {13, 3, 14}, {9, 3, 16},
            {25, 3, 15}, {28, 3, 16}, {29, 3, 16}, {30, 3, 16}, {4, 3, 13},
            {8, 3, 13}, {10, 3, 14}, {14, 3, 13}, {18, 3, 13}, {22, 3, 13},
            {24, 3, 13}, {1, 3, 13}, {2, 3, 14}, {15, 3, 14}, {27, 3, 16},
            {31, 3, 16}
        };

        // Ordenar los dias por temperatura minima (del mas frio al mas caliente)
        for (int i = 0; i < 30; i++) {
            for (int j = i + 1; j < 31; j++) {
                if (dias[i].tempMinima > dias[j].tempMinima) {
                    Dia temp = dias[i];
                    dias[i] = dias[j];
                    dias[j] = temp;
                }
            }
        }

        // Mostrar los dias ordenados
        cout << "\nDias ordenados por temperatura minima (del mas frio al mas caliente):\n";
        for (int i = 0; i < 31; i++) {
            cout << "Dia " << dias[i].numero << " - Temperatura: " << dias[i].tempMaxima << " / "
                 << dias[i].tempMinima << " C" << endl;
        }
    }
}

// Funcion principal del programa. Solicita al usuario la palabra a buscar y la palabra de reemplazo.
int main() {
    string palabraBuscar, palabraReemplazar;
    
    // Solicitar al usuario la palabra a buscar y la palabra de reemplazo
    cout << "Ingrese la palabra a buscar: ";
    cin >> palabraBuscar;
    cout << "Ingrese la palabra de reemplazo: ";
    cin >> palabraReemplazar;
    
    // Convertir las palabras a minusculas para hacer la busqueda insensible a mayusculas/minusculas
    transform(palabraBuscar.begin(), palabraBuscar.end(), palabraBuscar.begin(), ::tolower);
    transform(palabraReemplazar.begin(), palabraReemplazar.end(), palabraReemplazar.begin(), ::tolower);
    
    // Procesar los archivos especificados
    procesarArchivo("clima_riobamba_enero_2025.txt", palabraBuscar, palabraReemplazar);
    procesarArchivo("operaciones.txt", palabraBuscar, palabraReemplazar);
    
    return 0;
}

