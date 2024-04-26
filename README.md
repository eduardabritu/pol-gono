# Sistema de calcular um polígono

Este programa em C calcula a área de um polígono dividindo-o em triângulos e somando as áreas de cada triângulo.

## codigo

#include <stdio.h>

#include <stdlib.h>


// Definindo a estrutura para representar um ponto
typedef struct {
    float X;
    float Y;
} Ponto;

// Função para calcular a área de um triângulo formado por três pontos A, B e C
float AreaTriangulo(Ponto A, Ponto B, Ponto C) {
    return abs((A.X * (B.Y - C.Y) + B.X * (C.Y - A.Y) + C.X * (A.Y - B.Y)) / 2.0);
}

// Função para ler os pontos do arquivo e calcular a área do polígono
float AreaPoligono(FILE *file) {
    int numVertices;
    fscanf(file, "%d", &numVertices);

    Ponto *vertices = (Ponto *)malloc(numVertices * sizeof(Ponto));

    // Lendo os vértices do arquivo
    for (int i = 0; i < numVertices; i++) {
        fscanf(file, "%f %f", &vertices[i].X, &vertices[i].Y);
    }

    // Calculando a área do polígono dividindo-o em triângulos
    float areaTotal = 0;
    for (int i = 1; i < numVertices - 1; i++) {
        areaTotal += AreaTriangulo(vertices[0], vertices[i], vertices[i + 1]);
    }

    free(vertices);

    return areaTotal;
}

int main() {
    FILE *file;
    file = fopen("poligonoABC.txt", "r");

    if (file == NULL) {
        printf("Erro ao abrir o arquivo.\n");
        perror("fopen");
    return 1;
    }

    float area = AreaPoligono(file);

    printf("A área do poligono e %.2f\n", area);

    fclose(file);
    return 0;
}
