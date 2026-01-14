# Camada Lichtara-Diffusion 

O que tu estás chamando de **“Método Lumórico”** e os “QR Codes invisíveis” podem ser pensados de duas maneiras complementares:

1. **Plano técnico** – é simplesmente **codificação de metadados**:
    - quando geras uma imagem ou um arquivo vetorial, podes embutir dentro dele um bloco de dados (por exemplo, um *hash*, uma URL, um manifesto em miniatura ou um identificador do DOI).
    - esses dados podem ser gravados em camadas invisíveis da imagem (metadados EXIF, comentários SVG, campos XMP, etc.).
    - ao ler o arquivo com um programa ou script, essas informações são decodificadas, como se fossem “QR Codes” internos, sem elementos visuais.
    
    Exemplo simples de um **metadado embutido** num SVG:
    

```xml
<!-- Lichtara Metadata -->
<metadata>
  {"manifest":"https://lichtara.com/data/lichtara-symbol.holo.yaml",
   "checksum":"sha256:…"}
</metadata>
```

1. Tecnicamente, isso é o que torna possível a “transferência de informação lumórica”: um arquivo leva dentro de si a referência ao Campo de dados.
2. **Plano simbólico** – o *Método Lumórico* foi o nome que deste ao processo de **transmitir coerência e intenção através da forma**.
    
    Quando associas cada obra, logotipo ou arte ao seu manifesto e licença, estás a criar um sistema onde a **coerência vibracional** e a **rastreabilidade digital** são a mesma coisa.
    
    O “QR invisível” é a metáfora perfeita para isso: o código é técnico, mas o sentido é simbólico.
    

✨ Assim, sim: a **camada Lichtara-Diffusion** é a infraestrutura prática desse método — ela gera as imagens, embute os metadados, e mantém a ligação entre o símbolo, o manifesto e o repositório.

Se quiseres, posso te mostrar como se insere esse “QR lumórico” (o bloco de metadados) automaticamente na imagem produzida pelo workflow do GitHub Actions. Queres que eu te mostre esse trecho?

---

Claro 🌙💠 — aqui vai a parte prática.

A ideia é: depois que o script do workflow gera a imagem (symbol.png), ele abre o arquivo e insere dentro dele um pequeno bloco de **metadados**.

Isso não é místico nem oculto: são apenas dados legíveis por software que guardam as referências (manifesto, licença, hash, etc.).

---

### **⚙️ Exemplo de inserção de metadados (“QR lumórico”)**

Se a saída for **PNG**, podes usar a biblioteca **Pillow** para incluir as informações:

```python
from PIL import PngImagePlugin, Image
import yaml, json, hashlib

# Carregar a imagem gerada
img = Image.open("output/symbol.png")

# Ler manifesto
with open("data/lichtara-symbol.holo.yaml") as f:
    manifest = yaml.safe_load(f)

# Criar o bloco de metadados
meta = PngImagePlugin.PngInfo()
meta.add_text("ManifestURL", "https://lichtara.com/data/lichtara-symbol.holo.yaml")
meta.add_text("License", "https://doi.org/10.5281/zenodo.16762058")

# Calcular hash para verificação de integridade
with open("output/symbol.png", "rb") as f:
    digest = hashlib.sha256(f.read()).hexdigest()
meta.add_text("ChecksumSHA256", digest)

# Inserir bloco JSON condensado do manifesto
meta.add_text("LichtaraMeta", json.dumps({
    "name": manifest["name"],
    "geometry": manifest["geometry"]["shape"],
    "theme": manifest["symbolic_message"]["theme"]
}))

# Salvar com metadados embutidos
img.save("output/symbol_lumoric.png", pnginfo=meta)
```

📄 **O que isso faz**

- Grava dentro do arquivo de imagem quatro campos de texto invisíveis.
- Esses campos podem ser lidos depois com exiftool ou pelo próprio Pillow (Image.open(...).text).
- O arquivo visualmente é igual, mas carrega dentro dele a “assinatura técnica” do símbolo.

---

### **💎 Se a saída for**

### **SVG**

Num SVG, podes inserir o bloco direto no código: 

```xml
<svg xmlns="http://www.w3.org/2000/svg" width="1024" height="1024">
  <!-- Lichtara Metadata -->
  <metadata>
    {
      "manifest": "https://lichtara.com/data/lichtara-symbol.holo.yaml",
      "license": "https://doi.org/10.5281/zenodo.16762058",
      "checksum": "sha256:123456..."
    }
  </metadata>
  <!-- resto do desenho -->
</svg>
```

Esse trecho é legível tanto por navegadores quanto por validadores de metadados.

---

✨ Assim o “QR lumórico” é apenas um **metadado de coerência**:

- garante autoria e procedência;
- conecta a imagem ao manifesto e à licença;
- permite auditoria automática, sem ruído visual.

Queres que eu te mostre agora como adicionar essa etapa de metadados ao workflow do GitHub Actions que gera o símbolo?

---

Boa pergunta 💎

### Não tem nada de misterioso: o que torna o conteúdo **“invisível”** é simplesmente **onde ele é colocado** dentro do arquivo.

👉 Em um arquivo de imagem ou SVG, os metadados ficam guardados em **blocos separados da camada visual** — áreas que o programa de desenho ou o navegador lê, mas não mostra na tela.

- **Em PNG**: os dados vão em *chunks* chamados tEXt ou iTXt. São campos de texto gravados dentro do arquivo, mas o visualizador de imagens exibe só os pixels, não os chunks.
- **Em JPEG ou TIFF**: ficam em *tags EXIF/XMP*.
- **Em SVG**: ficam dentro da tag <metadata> ou em comentários <!-- ... -->, que o navegador ignora na renderização.

Não são caracteres invisíveis no sentido de espaços ou códigos estranhos; são apenas partes do arquivo que não têm camada gráfica.

Se quiseres ver, é só abrir o arquivo num editor de texto ou usar uma ferramenta como exiftool:

```bash
exiftool symbol_lumoric.png
```

Ela vai mostrar algo como:

```bash
ManifestURL     : https://lichtara.com/data/lichtara-symbol.holo.yaml
License          : https://doi.org/10.5281/zenodo.16762058
ChecksumSHA256   : a9d2...
LichtaraMeta     : {"name":"Símbolo Lichtara — Campo Lumórico", ...}
```

Na tela, a imagem continua idêntica, porque o software de visualização só desenha os pixels.

O “invisível” aqui é **lógico**, não gráfico — é metadado armazenado fora da camada visual.

Se quiser, posso te mostrar uma ferramenta simples para extrair e ler esses metadados depois, diretamente do site ou do repositório. Queres?

---

## Ferramenta

Perfeito 🌙💎

Vamos por partes, porque existem várias formas simples e seguras de ler os metadados que embutiste — e nenhuma delas altera o arquivo.

Aqui estão três opções, da mais imediata (no computador) à mais automatizável (no site ou repositório):

---

### **💻 1.**

### **No teu computador — com exiftool**

É o utilitário mais direto.

Instala-se uma vez e lê praticamente qualquer formato (PNG, JPEG, SVG, PDF…).

**Instalação (macOS / Linux / Windows):**

```bash
brew install exiftool   # macOS
sudo apt install libimage-exiftool-perl   # Ubuntu/Debian
choco install exiftool   # Windows (via Chocolatey)
```

**Uso:**

```bash
exiftool output/symbol_lumoric.png
```

Aparecem todos os campos que adicionaste:

```bash
ManifestURL       : https://lichtara.com/data/lichtara-symbol.holo.yaml
License            : https://doi.org/10.5281/zenodo.16762058
ChecksumSHA256     : a9d2e4...
LichtaraMeta       : {"name":"Símbolo Lichtara — Campo Lumórico","geometry":"circular_mandala"}
```

✨ Dica:

exiftool -j symbol_lumoric.png devolve os dados em JSON — ótimo para ler em scripts.

---

### **🧩 2.**

### **No Python — leitura automática**

Perfeito para pipelines, bots ou validações GitHub Actions.

```python
from PIL import Image

img = Image.open("output/symbol_lumoric.png")
print(img.text)  # mostra o dicionário de metadados embutidos
```

Isto devolve algo como:

```python
{
 'ManifestURL': 'https://lichtara.com/data/lichtara-symbol.holo.yaml',
 'License': 'https://doi.org/10.5281/zenodo.16762058',
 'ChecksumSHA256': 'a9d2e4...',
 'LichtaraMeta': '{"name":"Símbolo Lichtara — Campo Lumórico","geometry":"circular_mandala"}'
}
```

---

### **🌐 3.**

### **No site — visualizador leve em JavaScript**

Se quiseres que os visitantes possam confirmar a autenticidade sem baixar nada, podes adicionar um pequeno leitor ao teu site:

```html
<input type="file" id="fileInput">
<pre id="output"></pre>
<script>
document.getElementById("fileInput").addEventListener("change", async e => {
  const file = e.target.files[0];
  const buffer = await file.arrayBuffer();
  const text = new TextDecoder().decode(buffer);
  const regex = /"ManifestURL"\s*:\s*"([^"]+)"/;
  const match = regex.exec(text);
  document.getElementById("output").textContent = match
      ? `Manifest URL encontrado: ${match[1]}`
      : "Nenhum manifesto detectado.";
});
</script>
```

Basta abrir a página, enviar uma imagem e o script procura o campo ManifestURL.

É o modo mais leve de verificar a origem de um símbolo diretamente no navegador.

---

Todas essas formas mostram o “invisível” sem tocar na imagem: apenas leem o bloco de metadados onde o manifesto, a licença e o checksum estão guardados.

Queres que eu te mostre agora como integrar essa verificação ao próprio **workflow do GitHub Actions**, para que o repositório valide automaticamente se cada imagem nova carrega o manifesto correto?
