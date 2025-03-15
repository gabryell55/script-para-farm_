# script-para-farm_-- Script para Mini City RP - Sistema de Empregos
local Players = game:GetService("Players")
local Jobs = {
    {Name = "Policial", Salary = 50},
    {Name = "Médico", Salary = 45},
    {Name = "Taxista", Salary = 30},
    {Name = "Lixeiro", Salary = 25},
}

local function giveSalary(player)
    if player and player:FindFirstChild("leaderstats") and player.leaderstats:FindFirstChild("Dinheiro") then
        local job = player:FindFirstChild("Job")
        if job then
            for _, j in pairs(Jobs) do
                if j.Name == job.Value then
                    player.leaderstats.Dinheiro.Value = player.leaderstats.Dinheiro.Value + j.Salary
                    break
                end
            end
        end
    end
end

local function setupPlayer(player)
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local money = Instance.new("IntValue")
    money.Name = "Dinheiro"
    money.Value = 100 -- Dinheiro inicial
    money.Parent = leaderstats

    local job = Instance.new("StringValue")
    job.Name = "Job"
    job.Value = "Desempregado"
    job.Parent = player
end

Players.PlayerAdded:Connect(setupPlayer)

while true do
    for _, player in pairs(Players:GetPlayers()) do
        giveSalary(player)
    end
    wait(60) -- Salário a cada minuto
end

GetService("Players")
local CollectionAreas = game.Workspace:FindFirstChild("CollectionAreas") -- Pasta com áreas de coleta
local PieceName = "Peça" -- Nome do item a ser coletado

local function setupPlayer(player)
    local leaderstats = Instance.new("Folder")
    leaderstats.Name = "leaderstats"
    leaderstats.Parent = player

    local pieces = Instance.new("IntValue")
    pieces.Name = "Peças"
    pieces.Value = 0 -- Quantidade inicial de peças
    pieces.Parent = leaderstats
end

local function collectPieces(player)
    if player and player.Character and CollectionAreas then
        for _, area in pairs(CollectionAreas:GetChildren()) do
            if (player.Character.PrimaryPart.Position - area.Position).Magnitude < 10 then -- Raio de coleta
                player.leaderstats.Peças.Value = player.leaderstats.Peças.Value + math.random(1, 5) -- Adiciona de 1 a 5 peças
            end
        end
    end
end

Players.PlayerAdded:Connect(setupPlayer)

while true do
    for _, player in pairs(Players:GetPlayers()) do
        collectPieces(player)
    end
    wait(5) -- Coleta a cada 5 segundos
end
