# projeto_desafio_alura_imersao_2

Tipo de problema que o código resolve:

Esse código resolve o problema de gerar automaticamente uma descrição textual de uma imagem. Ele utiliza um modelo de inteligência artificial generativa para analisar a imagem e criar um texto que descreva seu conteúdo.

Esse tipo de código pode ser útil em diversas aplicações, como:

Acessibilidade: Gerar descrições de imagens para pessoas com deficiência visual.
Organização de imagens: Automaticamente classificar e rotular imagens em um banco de dados.
Geração de conteúdo: Criar legendas para fotos em redes sociais.


DESCRIÇÃO DO CÓDIGO

!pip install -U google-generativeai
Este comando instala a biblioteca google-generativeai via pip, que é necessária para usar os modelos de geração de conteúdo do Google AI.
2. Importações:

Python
import google.generativeai as genai
from google.colab import userdata
import PIL.Image
content_copy
As seguintes bibliotecas são importadas:
genai: A biblioteca principal do Google Generative AI.
userdata: Para acessar dados do usuário armazenados no Google Colab.
PIL.Image: Para carregar e processar imagens.
3. Autenticação:

Python
api_key = userdata.get('GOOGLE_API_KEY')
genai.configure(api_key=api_key)
content_copy
A chave da API do Google Cloud Platform (GCP) é recuperada do armazenamento do usuário no Colab (userdata.get('GOOGLE_API_KEY')).
A chave API é configurada para a biblioteca genai usando genai.configure().
4. Listar Modelos Disponíveis:

for m in genai.list_models():
    if 'generateContent' in m.supported_generation_methods:
        print(m.name)
content_copy
Este loop percorre todos os modelos disponíveis na API genai.
A condição if 'generateContent' in m.supported_generation_methods: verifica se o modelo suporta a geração de conteúdo a partir de imagens.
Se o modelo suportar, seu nome é impresso.
5. Selecionar Modelo:

model = genai.GenerativeModel('gemini-pro-vision')
content_copy
A linha cria uma instância do modelo gemini-pro-vision, que é um modelo de geração de conteúdo de última geração do Google AI.
6. Obter Caminho da Imagem do Usuário:

def get_user_image_path():
    """Prompts the user to enter the image URL or local path."""
    while True:
        image_path = input("Enter the image URL or local path (e.g., /content/my_image.jpg): ")
        if image_path.startswith("http") or image_path.startswith("/"):
            return image_path
        else:
            print("Invalid path. Please enter a valid URL or local path.")
content_copy
A função get_user_image_path() é definida para solicitar ao usuário o caminho da imagem (URL ou caminho local).
Um loop while True é usado para garantir que o usuário insira um caminho válido.
O código verifica se o caminho começa com http (URL) ou / (caminho local).
Se o caminho for válido, ele é retornado; caso contrário, uma mensagem de erro é exibida e o loop continua.
7. Carregar Imagem:

Python
image_path = get_user_image_path()

try:
    # Attempt to open the image using the provided path
    img = PIL.Image.open(image_path)
except FileNotFoundError:
    print(f"Error: Image not found at '{image_path}'.")
    exit(1)
content_copy
O caminho da imagem obtido da função get_user_image_path() é armazenado em image_path.
Um bloco try-except é usado para tentar abrir a imagem usando o caminho fornecido.
Se a imagem for encontrada, ela é carregada como um objeto PIL.Image e armazenada em img.
Se a imagem não for encontrada, uma mensagem de erro é exibida (print(f"Error: Image not found at '{image_path}'.")) e o script sai com o código de erro 1 (exit(1)).
8. Geração de Conteúdo - Descrição Geral:

response = model.generate_content(img)
print("Resposta 1:", response.text)
content_copy
A linha gera conteúdo a partir da imagem img usando o modelo model e armazena a resposta em response.
O texto da resposta (response.text) é impresso como "Resposta 1".
9. Geração de Conteúdo - Resposta à Pergunta:

Python
response = model.generate_content(["Descreva a imagem ", img])
response.resolve()
