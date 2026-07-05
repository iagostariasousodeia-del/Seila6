-- ================================================
-- MATRIX v3.8 - REDZ HUB STYLE (CIANO)
-- ================================================

local Library = loadstring(game:HttpGet("https://raw.githubusercontent.com/iagostariasousodeia-del/Seila7/refs/heads/main/README.md"))()

-- Customização de Cores (Ciano / Redz Hub Style)
local Win = Library:MakeWindow({
    Title = "MATRIX HUB",
    SubTitle = "Redz Edition",
    Size = {Width = 400, Height = 350}
})

-- Criando a Aba Principal
local Tab = Win:MakeTab({Name = "Auto Farm"})

-- Criando o Submenu (Seção) chamado Auxiliar Nível
Tab:MakeSection("Auxiliar Nível")

-- Criando o Botão Ciano com o seu Loadstring
Tab:MakeButton({
    Name = "Auto Farme",
    Callback = function()
        -- Executa o seu script do GitHub
        loadstring(game:HttpGet("https://raw.githubusercontent.com/iagostariasousodeia-del/Seila7/refs/heads/main/README.md"))()
        
        -- Notificação estilo Redz
        Win:Notify({
            Content = "Auto Farm Ativado!",
            Duration = 3
        })
    end
})

-- Adicionando um enfeite para parecer mais com o Redz Hub
Tab:MakeSection("Configurações")

Tab:MakeToggle({
    Name = "Anti-Lag (FPS Boost)",
    Default = false,
    Callback = function(v)
        if v then
            print("Limpando texturas...")
        end
    end
})

-- Notificação de Inicialização
Win:Notify({
    Content = "MATRIX v3.8 Carregado | Ciano Theme",
    Duration = 5
})
        
        
