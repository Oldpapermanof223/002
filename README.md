-- SimpleHub: Um script Lua para Roblox inspirado em funcionalidades de hubs como Hoho Hub
-- Este é um exemplo educacional e não deve ser usado para violar os Termos de Serviço do Roblox

-- Carrega a biblioteca Rayfield para interface gráfica
local Rayfield = loadstring(game:HttpGet('https://raw.githubusercontent.com/shlexware/Rayfield/main/source'))()

-- Cria a janela principal da interface
local Window = Rayfield:CreateWindow({
    Name = "SimpleHub",
    LoadingTitle = "SimpleHub - Carregando...",
    LoadingSubtitle = "by xAI Example",
    ConfigurationSaving = {
        Enabled = true,
        FolderName = "SimpleHubConfig",
        FileName = "Config"
    },
    Discord = {
        Enabled = false
    }
})

-- Variáveis globais
local Players = game:GetService("Players")
local LocalPlayer = Players.LocalPlayer
local TweenService = game:GetService("TweenService")
local Workspace = game:GetService("Workspace")

-- Função para teleporte simples
local function teleportTo(position)
    if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("HumanoidRootPart") then
        LocalPlayer.Character.HumanoidRootPart.CFrame = CFrame.new(position)
    end
end

-- Função para auto-farm (exemplo: coletar itens próximos)
local function autoFarm()
    local isFarming = false
    return function()
        isFarming = not isFarming
        if isFarming then
            Rayfield:Notify({
                Title = "Auto-Farm",
                Content = "Auto-Farm ativado!",
                Duration = 3
            })
            while isFarming do
                for _, item in pairs(Workspace:GetDescendants()) do
                    if item:IsA("BasePart") and item.Name == "CollectableItem" then -- Exemplo de item coletável
                        teleportTo(item.Position + Vector3.new(0, 5, 0))
                        task.wait(0.5)
                    end
                end
                task.wait(1)
            end
        else
            Rayfield:Notify({
                Title = "Auto-Farm",
                Content = "Auto-Farm desativado!",
                Duration = 3
            })
        end
    end
end

-- Aba principal
local MainTab = Window:CreateTab("Principal", 4483362458) -- Ícone opcional

-- Botão para teleporte a um NPC específico
MainTab:CreateButton({
    Name = "Teleportar para NPC Exemplo",
    Callback = function()
        -- Substitua pelo CFrame ou posição do NPC desejado
        local npcPosition = Vector3.new(0, 10, 0) -- Exemplo de coordenada
        teleportTo(npcPosition)
        Rayfield:Notify({
            Title = "Teleporte",
            Content = "Teleportado para o NPC!",
            Duration = 3
        })
    end
})

-- Toggle para auto-farm
MainTab:CreateToggle({
    Name = "Ativar Auto-Farm",
    CurrentValue = false,
    Callback = autoFarm()
})

-- Aba de configurações
local SettingsTab = Window:CreateTab("Configurações", 4483362458)

-- Slider para ajustar velocidade do personagem
SettingsTab:CreateSlider({
    Name = "Velocidade do Personagem",
    Range = {16, 100},
    Increment = 1,
    CurrentValue = 16,
    Callback = function(value)
        if LocalPlayer.Character and LocalPlayer.Character:FindFirstChild("Humanoid") then
            LocalPlayer.Character.Humanoid.WalkSpeed = value
            Rayfield:Notify({
                Title = "Configuração",
                Content = "Velocidade ajustada para " .. value,
                Duration = 3
            })
        end
    end
})

-- Função de inicialização
local function init()
    Rayfield:Notify({
        Title = "SimpleHub",
        Content = "Script carregado com sucesso! Use as abas para explorar as funcionalidades.",
        Duration = 5
    })
end

-- Executa a inicialização
init()

-- Proteção básica contra execução múltipla
if _G.SimpleHubLoaded then
    Rayfield:Notify({
        Title = "Erro",
        Content = "SimpleHub já está carregado!",
        Duration = 5
    })
    return
end
_G.SimpleHubLoaded = true
