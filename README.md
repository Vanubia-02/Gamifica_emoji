# 👾 Caça ao tesouro de emojis 👾

Emoji Scavenger Hunt é um jogo experimental baseado na web que usa TensorFlow.js para identificar objetos vistos por sua webcam ou câmera móvel no navegador. Mostramos emojis 🍌 ⏰ ☕️ 📱 e você deve encontrar esses objetos no mundo real antes que o tempo acabe 🏆 👍.

Descubra como construímos esse experimento lendo nosso [artigo](https://medium.com/tensorflow/a-look-at-how-we-built-the-emoji-scavenger-hunt-using-tensorflow-js- 3d760a7ebfe6) no blog do Tensorflow ou experimente você mesmo em [g.co/emojiscavengerhunt](http://g.co/emojiscavengerhunt).
## Development

```sh
yarn prep
```

Executar `yarn prep` usará o fio para obter os pacotes corretos e configurar as pastas corretas. Se você não possui o [yarn](https://yarnpkg.com/lang/en/docs/install/), você pode instalá-lo via homebrew (para Mac). Se você já está executando node/npm com nvm (nossa recomendação), você pode instalar o yarn sem node usando `brew install yarn --without-node`.

Para iniciar o desenvolvimento local, também exigimos a instalação do [Google Cloud SDK](https://cloud.google.com/sdk/downloads) e dos [componentes do App Engine](https://cloud.google.com) associados /appengine/docs/standard/python/download). Eles são usados ​​para o servidor da web local e para envio ao mecanismo de aplicativo para hospedagem de site estático.

Depois de instalar ambos, você pode executar o servidor de desenvolvimento local com:

```sh
yarn dev
```

Esta tarefa usa `watchify` para observar continuamente alterações nos arquivos JS e SASS e recompila-los se alguma alteração for detectada. Você pode acessar o servidor de desenvolvimento local em `http://localhost:3000/`

Ao construir ativos para uso em produção:

```sh
yarn build
```

Isso reduzirá o SASS e o JS para servir na produção.

## Construa seu próprio modelo
Você pode construir seu próprio modelo de reconhecimento de imagem executando um contêiner Docker.
Dockerfiles estão no diretório `training`.

Prepare imagens para treinamento dividindo-as em diretórios para cada etiqueta
nome que você deseja treinar.
Por exemplo: a estrutura de diretórios para treinamento *cat* e *dog* será semelhante a
segue assumindo que os dados da imagem são armazenados em `data/images`.

```
data
└── images
    ├── cat
    │   ├── cat1.jpg
    │   ├── cat2.jpg
    │   └── ...
    └── dog
        ├── dog1.jpg
        ├── dog2.jpg
        └── ...
```

Quando as imagens de amostra estiverem prontas, você poderá iniciar o treinamento criando e
executando o contêiner Docker.

```
$ cd training
$ docker build -t model-builder .
$ docker run -v /path/to/data:/data -it model-builder
```

Após a conclusão do treinamento, você verá três arquivos na área
Diretório `data/saved_model_web`:

- tensorflowjs_model.pb (the dataflow graph)
- weights_manifest.json (weight manifest file)
- group1-shard\*of\* (collection of binary weight files)

Eles são arquivos SavedModel em um formato amigável para web, convertidos pelo
[Conversor TensorFlow.js](https://github.com/tensorflow/tfjs-converter).
Você pode criar seu próprio jogo usando seu próprio modelo de reconhecimento de imagem personalizado, substituindo
os arquivos correspondentes no diretório `dist/model/` com os recém-gerados.

O script de treinamento também irá gerar um arquivo chamado `scavenger_classes.ts`
que funciona em conjunto com seu modelo personalizado gerado.
Você precisa substituir o arquivo em `src/js/scavenger_classes.ts` por este novo
gerou o arquivo `scavenger_classes.ts` para que os rótulos do seu modelo correspondam
com os dados treinados.
Depois de substituir o arquivo você pode executar o script de construção normalmente para testar seu
modelo em um navegador. Consulte o arquivo README para obter informações sobre como executar uma visualização
servidor.

Atualize a lógica do jogo em `src/js/game.ts` se necessário.

### Usando GPU
Você pode aumentar a velocidade de treinamento utilizando sua GPU.
Se você quiser usar a GPU para treinamento, instale
[nvidia-docker](https://github.com/NVIDIA/nvidia-docker) e execute:
```
$ cd training
$ nvidia-docker build -f Dockerfile.gpu model-builder
$ nvidia-docker run -v /path/to/data:/data -it model-builder
```

## Licença

Direitos autorais 2018 Google LLC

Licensed under the Apache License, Version 2.0 (the "License");
you may not use this file except in compliance with the License.
You may obtain a copy of the License at

https://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software
distributed under the License is distributed on an "AS IS" BASIS,
WITHOUT WARRANTIES OR CONDITIONS OF ANY KIND, either express or implied.
See the License for the specific language governing permissions and
limitations under the License.

## Credits

This is an experiment and collaboration between Google Brand Studio and the [PAIR](https://ai.google/pair/) teams at Google.

## Final Thoughts

This is not an official Google product. We will do our best to support and maintain this experiment but your mileage may vary.

We encourage open sourcing projects as a way of learning from each other. Please respect our and other creators’ rights, including copyright and trademark rights when present, when sharing these works and creating derivative work.

If you want more info on Google's policy, you can find that [here](https://policies.google.com/)
