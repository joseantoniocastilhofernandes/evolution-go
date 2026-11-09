# Evolution Go — build do LionCRM

Fork do `evolution-foundation/evolution-go` mantido para o LionCRM. Branch de trabalho: `lioncrm`.

## Por que existe

Em 02/11/2026 o WhatsApp passou a recusar a versão de cliente embutida na whatsmeow usada pela 0.7.2
(`2.3000.1042386815`) com `Client outdated (405)`, e o QR code deixou de ser gerado. A 0.7.2 buscava a
versão atual do WhatsApp Web, mas só gravava em `DeviceProps`; o handshake continuava com a versão antiga.

Correção deste fork: `applyHandshakeWhatsAppVersion` em `pkg/whatsmeow/service/whatsmeow.go` chama
`store.SetWAVersion` com a versão atual antes de cada conexão. O log mostra
`Handshake WhatsApp version: ...` a cada início de cliente.

## Imagem

Publicada pelo workflow `.github/workflows/publish_lioncrm_image.yml` a cada push no branch `lioncrm`:

- `ghcr.io/joseantoniocastilhofernandes/evolution-go:<versão>-lioncrm-<commit>` → usar esta (fixa) no YAML
- `ghcr.io/joseantoniocastilhofernandes/evolution-go:<versão>-lioncrm` → atalho para a última da série

## Emergência: forçar a versão sem rebuild

Se o WhatsApp exigir uma versão e a busca automática falhar, defina no container e reinicie:

```
WHATSAPP_VERSION_MAJOR=2
WHATSAPP_VERSION_MINOR=3000
WHATSAPP_VERSION_PATCH=<client_revision atual do WhatsApp Web>
```

Remova as variáveis depois, para voltar à busca automática.

## Quando só a versão não resolver

Se o WhatsApp mudar o protocolo (não só a versão), é preciso atualizar a dependência `go.mau.fi/whatsmeow`
(e possivelmente a versão do Go no `Dockerfile`), fazer push no branch `lioncrm` e trocar a tag no YAML.
