import numpy as np
import cv2
import time
import sys
from google.colab import files
from google.colab.patches import cv2_imshow
import io
from PIL import Image

# --- Função de Teste Modificada (Apenas OpenCV) ---

def visualTest(color_img, img, maxCor, thresh, dst):
    """
    Executa a detecção de cantos com a função nativa (cv2)
    e exibe os resultados no Colab.
    """

    # Teste da implementação nativa (Built-in Shi-Tomasi)
    try:
        print("\nExecutando função 'cv2.goodFeaturesToTrack'...")
        start_time = time.time()
        # Garante que os argumentos numéricos sejam tipos Python nativos
        builtInCorners_float = cv2.goodFeaturesToTrack(img, int(maxCor), float(thresh), int(dst))
        end_time = time.time()

        if builtInCorners_float is not None:
            builtInCorners = np.int32(builtInCorners_float)

            print(f"Função nativa encontrou {len(builtInCorners)} cantos em {end_time - start_time:.6f} segundos.")

            tmpImage_builtin = color_img.copy()
            for corner in builtInCorners:
                x, y = corner.ravel()
                cv2.circle(tmpImage_builtin, (x, y), 3, (0, 255, 0), -1) # Cantos em Verde

            print("Resultado de 'cv2.goodFeaturesToTrack':")
            cv2_imshow(tmpImage_builtin)
        else:
            print("Função nativa (cv2.goodFeaturesToTrack) não encontrou cantos.")

    except Exception as e:
        print(f"Erro ao executar 'cv2.goodFeaturesToTrack': {e}")

# --- Funções Auxiliares do Colab ---

def load_image_from_upload(file_dict):
    """
    Carrega uma imagem a partir de um arquivo enviado via files.upload().
    Retorna a imagem em formato BGR (padrão OpenCV).
    """
    if not file_dict:
        print("Nenhuma imagem enviada.")
        return None

    try:
        # Pega o primeiro arquivo enviado
        filename = list(file_dict.keys())[0]
        file_data = file_dict[filename]

        # Converte os bytes para uma imagem PIL
        pil_image = Image.open(io.BytesIO(file_data))

        # Converte a imagem PIL para um array NumPy
        # PIL (RGB ou RGBA) -> OpenCV (BGR)
        color_img_np = np.array(pil_image)

        if len(color_img_np.shape) == 3 and color_img_np.shape[2] == 4:
            # Converte RGBA para BGR
            color_img = cv2.cvtColor(color_img_np, cv2.COLOR_RGBA2BGR)
        elif len(color_img_np.shape) == 3:
            # Converte RGB para BGR
            color_img = cv2.cvtColor(color_img_np, cv2.COLOR_RGB2BGR)
        elif len(color_img_np.shape) == 2:
            # Imagem já é P&B, converte para BGR para exibição
            color_img = cv2.cvtColor(color_img_np, cv2.COLOR_GRAY2BGR)
        else:
            print(f"Formato de imagem não suportado: {color_img_np.shape}")
            return None

        print(f"Imagem '{filename}' carregada com sucesso.")
        return color_img

    except Exception as e:
        print(f"Falha ao carregar a imagem: {e}")
        return None

# --- Função Principal ---

def main():
    # --- 1. Upload e carregamento da Imagem de Teste ---
    print("Por favor, envie sua imagem de teste (ex: .png, .jpg)...")
    uploaded_image = files.upload()
    color_img = load_image_from_upload(uploaded_image)

    if color_img is None:
        print("Não é possível continuar sem uma imagem válida.")
        return

    # --- 2. Preparação da Imagem ---

    # 'img' deve ser a versão em escala de cinza para detecção
    if len(color_img.shape) == 3:
        img = cv2.cvtColor(color_img, cv2.COLOR_BGR2GRAY)
    else:
        # Se já for P&B, 'img' é uma cópia
        img = color_img.copy()
        # E 'color_img' precisa ser convertida para 3 canais para desenhar colorido
        color_img = cv2.cvtColor(color_img, cv2.COLOR_GRAY2BGR)

    # --- 3. Definição dos Parâmetros ---
    maxCorners = 3000 #quantidade de cantos
    thresh = 0.01 #limiar relativo de qualidade
    dist = 10  #distância mínima entre cantos

    # --- 4. Execução do Teste Visual ---
    visualTest(color_img, img, maxCorners, thresh, dist)

if __name__ == "__main__":
    main()