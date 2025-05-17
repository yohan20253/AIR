-- Hitbox Estendida 800x600 com Dano ao ser tocada por "Bullet"
-- Script para testes locais no Roblox Studio

local Players = game:GetService("Players")

-- Configurações
local QUADRO_SIZE = Vector3.new(8, 6, 0.2) -- 800x600 equivalente em studs
local DAMAGE = 20

-- Cria a hitbox invisível para o personagem do jogador
local function criarHitbox(player)
	player.CharacterAdded:Connect(function(character)
		local root = character:WaitForChild("HumanoidRootPart")

		local hitbox = Instance.new("Part")
		hitbox.Name = "HitboxQuadro"
		hitbox.Size = QUADRO_SIZE
		hitbox.Transparency = 1
		hitbox.CanCollide = false
		hitbox.CanTouch = true
		hitbox.Anchored = false
		hitbox.Massless = true
		hitbox.Parent = character

		local weld = Instance.new("WeldConstraint")
		weld.Part0 = hitbox
		weld.Part1 = root
		weld.Parent = hitbox

		hitbox.CFrame = root.CFrame * CFrame.new(0, 0, -2)

		hitbox.Touched:Connect(function(hit)
			if hit and hit:IsDescendantOf(character) == false and hit.Name == "Bullet" then
				local humanoid = character:FindFirstChildOfClass("Humanoid")
				if humanoid and humanoid.Health > 0 then
					humanoid:TakeDamage(DAMAGE)
				end
			end
		end)
	end)
end

-- Aplica hitbox a todos os jogadores (inimigos)
for _, player in ipairs(Players:GetPlayers()) do
	criarHitbox(player)
end

Players.PlayerAdded:Connect(criarHitbox)
