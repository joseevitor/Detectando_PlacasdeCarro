# Detecção e Leitura de Placas Veiculares com Visão Computacional (OCR)

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/joseevitor/Detectando_PlacasdeCarro/blob/main/Placas.ipynb)
![Python](https://img.shields.io/badge/Python-3.x-blue?logo=python&logoColor=white)
![OpenCV](https://img.shields.io/badge/OpenCV-Computer%20Vision-5C3EE8?logo=opencv&logoColor=white)
![Tesseract OCR](https://img.shields.io/badge/Tesseract-OCR-4B8BBE)
![Status](https://img.shields.io/badge/Status-Em%20evolução-yellow)

Pipeline completo de **Visão Computacional** e **OCR (Reconhecimento Óptico de Caracteres)** para localizar automaticamente a placa de um veículo em uma imagem, isolar a região de interesse e extrair o texto (placa) usando **Tesseract OCR**, com pós-processamento via **expressões regulares** para validar e corrigir o resultado final.

O projeto foi construído de forma incremental, testando e comparando diferentes técnicas clássicas de processamento de imagem (thresholding, morfologia, detecção de bordas/contornos, transformação de perspectiva) antes de chegar em uma solução mais robusta.



## Habilidades e Tecnologias Demonstradas

| Categoria | Tecnologias / Técnicas |
|---|---|
| **Linguagem** | Python |
| **Bibliotecas** | OpenCV (`cv2`), Pytesseract, NumPy, Matplotlib, Seaborn, scikit-image |
| **OCR** | Tesseract OCR (modelo treinado em português - `por.traineddata`), configuração de PSM (Page Segmentation Mode) |
| **Processamento de Imagem** | Conversão de espaço de cor (BGR → Grayscale), Suavização (Gaussian Blur) |
| **Binarização / Thresholding** | Threshold simples, Threshold adaptativo (Mean e Gaussian), Threshold de Otsu |
| **Morfologia Matemática** | Erosão, Dilatação, Abertura, Fechamento, Gradiente Morfológico, Top Hat, Black Hat |
| **Detecção de Bordas e Contornos** | Canny Edge Detection, `findContours`, `approxPolyDP`, `convexHull`, filtragem por área e proporção (aspect ratio) |
| **Geometria / Transformações** | Transformação de perspectiva (`getPerspectiveTransform`, `warpPerspective`), ordenação de pontos de um quadrilátero |
| **Realce de Bordas** | Operador Sobel para detecção de gradientes horizontais |
| **Segmentação** | Criação e aplicação de máscaras binárias, remoção de ruído nas bordas (`clear_border`) |
| **Pós-processamento de Texto** | Expressões regulares (`re`) para validar padrões de placa (Mercosul e antigo), correção heurística de caracteres ambíguos (ex.: `D` ↔ `0`) |
| **Ambiente** | Google Colab, instalação e configuração de dependências via linha de comando (`apt`, `pip`, `wget`) |
| **Boas práticas exploradas** | Análise exploratória visual (histograma de intensidade de pixels), comparação empírica entre técnicas, documentação incremental do raciocínio |



## Visão Geral do Pipeline

O notebook percorre, passo a passo, a evolução de uma solução de detecção de placas do zero:

1. **Configuração do ambiente**
   Instalação do Tesseract OCR, do pacote `pytesseract` e download do modelo de linguagem em português.

2. **Pré-processamento da imagem**
   Conversão para escala de cinza e experimentação com diferentes técnicas de binarização (simples, adaptativa e Otsu) para preparar a imagem para o OCR.

3. **Operações morfológicas**
   Aplicação de erosão, dilatação, abertura, fechamento, gradiente, top hat e black hat para realçar a região da placa e remover ruídos.

4. **Detecção da região da placa**
   - Detecção de bordas com **Canny**.
   - Extração de contornos e filtragem por **área** e **proporção largura/altura** (padrão de placas).
   - Aproximação poligonal (`approxPolyDP`) para identificar quadriláteros candidatos.

5. **Correção de perspectiva**
   Uso de `getPerspectiveTransform` e `warpPerspective` para "endireitar" a placa capturada em ângulo, com recorte automático de bordas/moldura residual.

6. **Realce de contraste e segmentação**
   Combinação de **Black Hat**, operador **Sobel**, thresholding de Otsu e máscaras binárias para isolar apenas os caracteres da placa.

7. **Extração de texto (OCR)**
   Aplicação do **Tesseract** com diferentes configurações de PSM sobre a imagem tratada.

8. **Validação e correção do texto extraído**
   Uso de **regex** para validar o padrão da placa e uma rotina de correção de caracteres frequentemente confundidos pelo OCR (ex.: `D`/`0`, `O`/`0`).

9. **Generalização**
   O pipeline é testado em múltiplas imagens (`placa.png`, `placa_carro2.png`, `placa_carro3.png`) para validar a robustez das técnicas em cenários diferentes.

---

## Estrutura

```
├── Placas.ipynb        # Notebook principal com todo o pipeline de detecção e OCR
└── README.md            # Este arquivo
```

## Como executar

O projeto foi desenvolvido para rodar no **Google Colab** (uso de `cv2_imshow`, `apt`, etc.), bastando clicar no badge acima. Para rodar localmente, é necessário adaptar as chamadas de instalação (`!apt`, `!pip`) para o seu ambiente e substituir `cv2_imshow` por `cv2.imshow` ou `matplotlib.pyplot.imshow`.

Dependências principais:
```bash
pip install opencv-python pytesseract numpy matplotlib seaborn scikit-image
```
E o Tesseract OCR instalado no sistema, com o pacote de idioma português (`por.traineddata`).

## Aprendizados registrados no notebook

Ao longo do desenvolvimento, os desafios e decisões técnicas foram documentados diretamente no notebook, incluindo tentativas que não funcionaram e o raciocínio por trás dos ajustes de parâmetros — reforçando um processo iterativo e experimental típico de projetos de visão computacional aplicada.

---

 *Projeto de estudo focado em Visão Computacional clássica, demonstrando domínio de técnicas fundamentais de processamento de imagem para resolver um problema real de OCR aplicado a placas veiculares.*
