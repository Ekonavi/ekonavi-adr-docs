# ADR 015: Implementação de vídeo e widget de upload multimídia

## Registro de alterações
* 27/08/2023: Rascunho inicial

## Status
ESBOÇO não implementado

## Abstrato
Este ADR propõe uma estratégia para implementar um widget de upload multimídia que permite a criação dinâmica de novos itens, suporta uploads de vídeos e imagens e fornece progresso de upload em tempo real. A implementação proposta usará um componente Global React para uploads, e cada item de mídia será armazenado como uma linha separada em um banco de dados Hasura PostgreSQL com uma coluna de classificação adicionada para ordem definida pelo usuário. Para rastrear o progresso do upload e lidar com uploads, usaremos React-Hook-Form junto com CloudFlare Stream Video para uploads de vídeo. O foco está nos uploads diretos do cliente, eliminando a necessidade de envolvimento dos trabalhadores.

## Contexto

A funcionalidade de upload de mídia existente em nosso aplicativo é ineficiente, complicada e carece de flexibilidade para lidar com vários tipos de mídia simultaneamente. Essa abordagem fragmentada leva a uma maior latência e afeta negativamente a experiência do usuário, especialmente quando os usuários carregam vários arquivos de mídia para uma única postagem ou interação.

Além disso, nosso sistema atual apresenta problemas de escalabilidade, dificultando a adaptação a novos formatos de mídia e ao aumento do tráfego. Um widget de upload de multimídia bem projetado poderia superar essas limitações, consolidando todas as opções de upload de mídia em um único e eficiente componente de UI que pode lidar com vários tipos de mídia.

Além disso, a análise competitiva mostrou que muitas plataformas líderes adotaram capacidades robustas de upload multimídia, estabelecendo um padrão de mercado que atualmente não atendemos. Portanto, implementar um widget de upload multimídia não é apenas uma adição de recurso, mas uma necessidade para permanecermos competitivos e melhorarmos nossas métricas de envolvimento do usuário.

Por fim, como nosso sistema depende da Cloudflare para diversos serviços, a compatibilidade com as tecnologias Cloudflare é essencial para qualquer solução que considerarmos. Este requisito aumenta ainda mais a urgência de uma abordagem reprojetada para uploads de mídia.


### Requisitos
1. O widget de upload deve permitir a criação dinâmica de novos itens.
2. Suporte para upload de vídeos e imagens.
3. Os uploads devem mostrar o progresso.
4. A mídia carregada deve ser classificável.
5. Cada item de mídia deve ter uma descrição.

### Pilha de tecnologia
- Próximo.js
- Datilografado
- Formulário React-Hook
-Hasura PostgreSQL
- Transmissão de vídeo CloudFlare

## Avaliação comparativa

Para determinar a melhor abordagem para um widget de upload multimídia, analisamos as práticas recomendadas do setor e as comparamos com nossos mecanismos de upload de mídia existentes. Também consideramos métricas qualitativas baseadas em evidências anedóticas e reclamações de usuários.

### Métricas de desempenho:

1. **Tempo de upload**:
     - Sistema Atual: Sujeito a atrasos e timeouts.
     - Padrão da indústria: uploads instantâneos com suporte CDN, como visto em plataformas como Instagram e Dropbox.
    
2. **Carga do servidor**:
     - Sistema Atual: Propenso a picos no uso da CPU.
     - Padrão da indústria: gerenciamento eficiente de carga do servidor por meio de processamento assíncrono, comumente visto em plataformas como o YouTube.

### Métricas para escalabilidade:

1. **Uploads simultâneos**:
     - Sistema Atual: Limitado pela arquitetura backend.
     - Padrão da indústria: capaz de lidar com centenas a milhares de uploads simultâneos, gerenciados por plataformas como o Google Fotos.

2. **Extensibilidade**:
     - Sistema Atual: Requer módulos separados para diferentes tipos de mídia.
     - Padrão da Indústria: Arquiteturas modulares e extensíveis, semelhantes às do Vimeo.

### Métricas para experiência do usuário:

1. **Tempo de interação do usuário**:
     - Sistema Atual: Múltiplas etapas necessárias para um único upload.
     - Padrão da indústria: uploads em uma única etapa com interfaces de arrastar e soltar, como visto no Dropbox e no Google Drive.

2. **Feedbacks e avaliações de usuários**:
     - Sistema Atual: O feedback do usuário indica espaço para melhorias.
     - Padrão do setor: altas taxas de satisfação relatadas para plataformas com widgets de upload eficientes, como o Slack.

Ao comparar o nosso sistema atual com os padrões da indústria e as principais plataformas, fica claro que há espaço significativo para melhorias. O widget de upload multimídia proposto visa preencher essa lacuna.

### Service Worker versus componente global para uploads
A abordagem do service worker foi considerada, mas descartada devido à complexidade adicional e ao suporte parcial do navegador. A abordagem do Global React Component foi escolhida por sua simplicidade e suporte completo ao navegador.

### Upload direto do cliente versus upload do trabalhador
O upload direto do cliente foi escolhido para lidar com todos os uploads de mídia, tanto imagens quanto vídeos, da mesma maneira. Isto serve principalmente para rastrear o progresso do upload de forma consistente em todos os tipos de mídia.

### Armazenamento de dados: linhas individuais versus matriz JSONB
Duas abordagens foram consideradas para armazenar itens de mídia no banco de dados Hasura PostgreSQL: armazenar cada item de mídia como uma linha individual ou armazenar todos os itens de mídia relacionados a uma entidade específica em uma única coluna do array JSONB.

#### Linhas individuais

##### Prós
1. **Flexibilidade:** Cada item de mídia pode ter seu próprio conjunto de metadados e pode ser consultado ou modificado individualmente.
2. **Melhor suporte para classificação:** Uma coluna de 'classificação' separada pode ser adicionada para manter a ordem de itens de mídia definida pelo usuário.
3. **Operações Atômicas:** É mais fácil lidar com a integridade transacional quando cada item de mídia é uma linha separada.

##### Contras
1. **Aumento do número de linhas:** Pode levar a mais linhas no banco de dados, o que pode afetar o desempenho da consulta se não for gerenciado adequadamente.

#### Matriz JSONB

##### Prós
1. **Simplicidade:** É mais fácil recuperar todos os itens de mídia relacionados a uma entidade em uma consulta.
2. **Número reduzido de linhas:** Diminui o número de linhas no banco de dados.

##### Contras
1. **Consultas complexas:** Consultas mais complexas são necessárias para manipular ou recuperar itens de mídia individuais.
2. **Atomicidade:** É mais difícil manter a integridade transacional ao modificar elementos individuais na matriz JSONB.

Após avaliar os prós e contras, optou-se por utilizar linhas individuais para armazenamento de cada item de mídia, principalmente pela flexibilidade que oferece e pela compatibilidade com classificação e integridade transacional.

## Comparação de implementação de upload: Service Worker vs componente Global React para uploads

### Abordagem do trabalhador de serviço

#### Prós:

1. **Processamento em segundo plano:** Service Workers podem lidar com uploads mesmo se o usuário sair da página ou fechá-la.
2. **Separação de preocupações:** A lógica de negócios para upload pode ser separada da UI, permitindo uma base de código mais limpa.
3. **Resiliência:** oferece a capacidade de tentar novamente uploads com falha automaticamente.

#### Contras:

1. **Suporte ao navegador:** Nem todos os navegadores oferecem suporte total a Service Workers.
2. **Complexidade:** A implementação de um Service Worker adiciona outra camada de complexidade ao seu aplicativo.
3. **Depuração:** A depuração de Service Workers pode ser um desafio devido ao seu ciclo de vida e escopo.

### Abordagem do Componente Global React

#### Prós:

1. **Simplicidade:** Mais fácil de implementar, com melhor suporte ao navegador.
2. **Uniformidade:** um método único para enviar arquivos de vídeo e imagem simplifica a manutenção do código.
3. **Feedback imediato:** Pode atualizar facilmente os componentes da UI em tempo real para mostrar progresso ou erros.

#### Contras:

1. **Bloqueio:** Pode bloquear o thread principal se não for implementado com cuidado, o que pode levar a uma experiência do usuário ruim.
2. **Gerenciamento de estado:** Mais lógica de front-end necessária para gerenciar o estado de cada elemento de mídia, incluindo erros e novas tentativas.
3. **Sem processamento em segundo plano:** Os uploads serão interrompidos se o usuário sair da página.


## Implementação da interface UI: considerações e sugestões

### Abordagem 1: Uploader de mídia com classificação e barra de progresso

#### Prós:
1. **Feedback imediato:** os usuários veem o progresso em tempo real.
2. **Melhor UX:** A classificação e as descrições de mídia melhoram o envolvimento do usuário.
3. **Envio geral mais rápido:** upload de vídeos e imagens enquanto o usuário preenche outras partes do formulário.

#### Contras:
1. **Complexidade:** Requer mais lógica de front-end para gerenciar o estado de cada elemento de mídia, incluindo erros e novas tentativas.
2. **Consistência do banco de dados:** Você precisará lidar com casos em que o upload de mídia falha após a criação do objeto pai.

### Abordagem 2: Formulário Multicampo Padrão com Indicação de Progresso

#### Prós:
1. **Implementação mais simples:** Mais fácil de construir.
2. **Consistência do banco de dados:** Todos os elementos de mídia são carregados juntos antes de criar o objeto pai.

#### Contras:
1. **Envio mais lento:** O usuário deve aguardar todos os uploads antes de enviar o formulário.
2. **Sem feedback em tempo real:** Falta de barras de progresso individuais para cada elemento de mídia.

### Decisão final

Depois de avaliar os prós e contras, optamos por um **Media Uploader com classificação e barra de progresso** para uma UX melhor, embora mais complexa. Em última análise, devemos encantar nossos usuários, mesmo que nossos desenvolvedores demorem o dobro do tempo para implementar.

## Estrutura de código sugerida

Dados os riscos e limitações potenciais de gerar um UUID no lado do cliente, como riscos de colisão e questões de segurança, recomendamos a criação primeiro de um stub post no servidor. O UUID do objeto será gerado e retornado do banco de dados, para ser preenchido no formulário à medida que o usuário o preencher.

### Stub Post e elementos de mídia no contexto de abordagens de UI

#### Abordagem 1: Uploader de mídia com classificação e barra de progresso

1. **Criar Stub Post**: Na inicialização do cliente, envie uma solicitação para criar um stub post. O servidor gera um UUID, armazena-o no banco de dados e o retorna ao cliente.
2. **Uploads de mídia individuais**: à medida que cada arquivo de mídia é selecionado ou arrastado para o carregador de mídia, carregue-o imediatamente no serviço de streaming de vídeo da Cloudflare usando o UUID retornado como identificador do objeto pai.
3. **Barra de progresso**: mostra uma barra de progresso ao lado de cada elemento de mídia para indicar seu status de upload.
4. **Classificação**: permite que o usuário classifique os elementos de mídia enquanto eles estão sendo carregados.

### Decisão final

Depois de avaliar os prós e contras, optamos por um **Media Uploader com classificação e barra de progresso** para uma UX melhor, embora mais complexa. Em última análise, devemos encantar nossos usuários, mesmo que nossos desenvolvedores demorem o dobro do tempo para implementar.

## Estrutura de código sugerida

Dados os riscos e limitações potenciais de gerar um UUID no lado do cliente, como riscos de colisão e questões de segurança, recomendamos a criação primeiro de um stub post no servidor. O UUID do objeto será gerado e retornado do banco de dados, para ser preenchido no formulário à medida que o usuário o preencher.

### Stub Post e elementos de mídia no contexto de abordagens de UI

#### Abordagem 1: Uploader de mídia com classificação e barra de progresso

1. **Criar Stub Post**: Na inicialização do cliente, envie uma solicitação para criar um stub post. O servidor gera um UUID, armazena-o no banco de dados e o retorna ao cliente.
2. **Uploads de mídia individuais**: à medida que cada arquivo de mídia é selecionado ou arrastado para o carregador de mídia, carregue-o imediatamente no serviço de streaming de vídeo da Cloudflare usando o UUID retornado como identificador do objeto pai.
3. **Barra de progresso**: mostra uma barra de progresso ao lado de cada elemento de mídia para indicar seu status de upload.
4. **Classificação**: permite que o usuário classifique os elementos de mídia enquanto eles estão sendo carregados.

#### Abordagem 2: Formulário Multicampo Padrão com Indicação de Progresso

1. **Criar stub post**: assim que o usuário começar a preencher o formulário, envie uma solicitação para criar um stub post. O servidor gera um UUID, armazena-o no banco de dados e o retorna ao cliente.
2. **Uploads de mídia individuais**: à medida que o usuário anexa mídia no formulário, coloque-os na fila para upload. Depois que o usuário enviar o formulário, inicie o processo de upload.
3. **Indicação de progresso**: Use um indicador circular de progresso na parte inferior do formulário para mostrar o progresso geral do upload.
4. **Sem classificação**: Esta abordagem não permite a classificação de mídia enquanto ela está sendo carregada, mas oferece uma interface mais simples.

### Pensamentos finais

Recomendamos fortemente a criação de um esboço de postagem pelos motivos mencionados acima. A abordagem 1 é ideal para casos de uso que exigem funcionalidades avançadas, como classificação em tempo real e monitoramento do progresso. A abordagem 2 é mais adequada para formulários mais simples onde a manipulação avançada de mídia não é necessária.

Essa estratégia alinha a geração de UUID do lado do servidor e uploads de mídia individuais dentro das implementações de UI, garantindo um processo de upload coerente e eficiente.


## Pacotes a serem usados

### React-Hook-Form
- Para validação e gerenciamento de formulários
- Facilita a implementação de campos de formulário dinâmicos para uploads de mídia

### React-DnD (arrastar e soltar)
- Para implementar a funcionalidade de classificação no uploader de mídia.
- Fornece uma UX melhor para reorganizar a mídia carregada.

### react-upload ou react-dropzone
- Para lidar com uploads de arquivos com progresso.
- Fornece suporte integrado para indicação de progresso.

###API CloudFlare
- Para gerar links de upload únicos e para uploads diretos para armazenamento CloudFlare.

## Sudocode da IU do carregador

```bash
npm install react-dropzone react-sortable-hoc react-hook-form @react95/progress-bar uuid
```

```typescript
import React, { useState } from 'react';
import { useFieldArray, useForm } from 'react-hook-form';
import { SortableContainer, SortableElement, SortableHandle } from 'react-sortable-hoc';
import Dropzone from 'react-dropzone';
import ProgressBar from '@react95/progress-bar';
import { v4 as uuidv4 } from 'uuid';

interface MediaUploadField {
  id: string;
  file: File | null;
  description: string;
  uploadProgress: number;
  // Add more fields if necessary
}

const DragHandle = SortableHandle(() => <span>::</span>);

const SortableItem = SortableElement(({ field }) => (
  <div>
    <DragHandle />
    <Dropzone onDrop={acceptedFiles => /* upload logic here */}>
      {({ getRootProps, getInputProps }) => (
        <div {...getRootProps()}>
          <input {...getInputProps()} />
          <p>Drag 'n' drop some files here, or click to select files</p>
        </div>
      )}
    </Dropzone>
    <input name={`media[${field.id}].description`} placeholder="Description" />
    <ProgressBar percent={field.uploadProgress} />
  </div>
));

const SortableList = SortableContainer(({ fields }) => {
  return (
    <div>
      {fields.map((field, index) => (
        <SortableItem key={field.id} index={index} field={field} />
      ))}
    </div>
  );
});

const MediaUploader: React.FC = () => {
  const { register, control, handleSubmit } = useForm<{ media: MediaUploadField[] }>();
  const { fields, append, remove, move } = useFieldArray({
    control,
    name: 'media'
  });

  const onSubmit = (data) => {
    // Handle the submit logic
  };

  const handleSortEnd = ({ oldIndex, newIndex }) => {
    move(oldIndex, newIndex);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <SortableList fields={fields} onSortEnd={handleSortEnd} useDragHandle={true} />
      <button type="button" onClick={() => append({ id: uuidv4(), file: null, description: '', uploadProgress: 0 })}>
        Add Media
      </button>
      <button type="submit">Submit</button>
    </form>
  );
};

export default MediaUploader;
```

## Componente global para lidar com o estado de upload

### Componente Global Uploader na árvore de componentes
O componente Global Uploader deve ser posicionado no alto da árvore de componentes, próximo à raiz do aplicativo, para garantir que ele possa ser facilmente acessado por diversas partes do aplicativo que possam exigir funcionalidades de upload de mídia.

### Acompanhamento de vários uploads e estado de progresso do XHR
O componente Global Uploader gerenciará vários uploads por meio de um objeto de estado interno onde cada upload recebe um identificador exclusivo (UUID). Este identificador será usado para rastrear e atualizar o progresso de cada upload através de eventos XMLHttpRequest (XHR).

### Risco de desmontar o componente
Se o usuário fechar a página ou sair, os uploads serão interrompidos. Para atenuar isso, uma mensagem "Tem certeza de que deseja sair? Os uploads estão em andamento". o alerta pop-up será mostrado quando o usuário tentar sair da página.

### Estrutura de código sugerida e pseudocódigo extenso

```ts

import React, { useState, useEffect } from 'react';
import { useForm } from 'react-hook-form';
import { v4 as uuidv4 } from 'uuid';

// Define your interface
interface UploadItem {
  id: string;
  file: File | null;
  progress: number;
}

const GlobalUploader: React.FC = () => {
  // State to track multiple uploads
  const [uploads, setUploads] = useState<Record<string, UploadItem>>({});

  // Hook form for other functionalities
  const { register, handleSubmit } = useForm();

  // Handle form submit
  const onSubmit = (data: any) => {
    // Implement submit logic
  };

  useEffect(() => {
    // Listen for the beforeunload event to warn the user
    const handleBeforeUnload = (event: BeforeUnloadEvent) => {
      if (Object.values(uploads).some(upload => upload.progress < 100)) {
        event.preventDefault();
        event.returnValue = "Are you sure you want to leave? Uploads are in progress.";
      }
    };

    window.addEventListener('beforeunload', handleBeforeUnload);
    return () => {
      window.removeEventListener('beforeunload', handleBeforeUnload);
    };
  }, [uploads]);

  // Trigger the upload
  const triggerUpload = (file: File) => {
    const uploadId = uuidv4(); // Unique identifier for the upload
    
    const xhr = new XMLHttpRequest();
    xhr.upload.onprogress = (event) => {
      const progress = (event.loaded / event.total) * 100;

      setUploads(prevUploads => ({
        ...prevUploads,
        [uploadId]: { id: uploadId, file, progress }
      }));
    };

    // Complete and error handling here
    xhr.onload = () => {
      // Complete logic
    };
    xhr.onerror = () => {
      // Error handling
    };

    xhr.open('POST', 'YOUR_UPLOAD_ENDPOINT_HERE', true);
    xhr.send(file);
  };

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      {/* Render your form fields here */}
      <input type="file" onChange={e => e.target.files && triggerUpload(e.target.files[0])} />
      <button type="submit">Submit</button>
    </form>
  );
};

export default GlobalUploader;
```

## Recursos

### Pilha de tecnologia e serviços
1. **Next.js**: [Site Oficial](https://nextjs.org/)
2. **TypeScript**: [Site Oficial](https://www.typescriptlang.org/)
3. **Formulário React-Hook**: [Pacote NPM](https://www.npmjs.com/package/react-hook-form)
4. **Hasura**: [Site Oficial](https://hasura.io/)
5. **PostgreSQL**: [Site Oficial](https://www.postgresql.org/)
6. **Transmissão de vídeo CloudFlare**: [Site oficial](https://www.cloudflare.com/products/stream/)
7. **Trabalhadores da CloudFlare**: [Site Oficial](https://workers.cloudflare.com/)
8. **CloudFlare KV**: [Site oficial](https://developers.cloudflare.com/workers/runtime-apis/kv)

### Pacotes React para upload de arquivos (para consideração)
1. **react-dropzone**: [Pacote NPM](https://www.npmjs.com/package/react-dropzone)
2. **react-uploady**: [Pacote NPM](https://www.npmjs.com/package/react-uploady)
3. **filepond-react**: [Pacote NPM](https://www.npmjs.com/package/filepond-react)

### Trabalhadores de serviço
1. **Documentos Web MDN sobre Service Workers**: [Guia MDN](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
