# === Default target: show help menu ===
all: help

# === Help menu ===
help:
	@echo ""
	@echo "-- Help Menu"
	@echo ""
	@echo "   make build          - build the Docker image"
	@echo "   make clean          - remove the Docker image"
	@echo "   make compose-build  - build using Docker Compose"
	@echo "   make compose-up     - start the stack (detached)"
	@echo "   make exec           - open a shell inside the running container"
	@echo "   make compose-down   - stop and clean up"
	@echo ""

# === Build Docker image directly ===
build:
	@docker build --tag booster_sim -f ./Dockerfile ..

# === Remove Docker image ===
clean:
	@docker rmi -f booster_sim

# === Build with Docker Compose (no cache) ===
compose-build:
	docker compose -f docker-compose.yml build --no-cache

# === Start the container stack in detached mode ===
compose-up:
	xhost +local:docker
	docker compose -f docker-compose.yml up -d

# === Open a bash shell inside the running container ===
exec:
	docker compose -f docker-compose.yml exec booster_sim bash

# === Stop and remove containers, networks, etc ===
compose-down:
	docker compose -f docker-compose.yml down

# === OPTIONAL: Previous run/clean recipes ===
# Run with Docker Compose (alternative interactive mode)
# compose-up:
# 	xhost +local:docker
# 	docker compose -f docker-compose.yml run --rm booster_sim

# Clean image (alternative)
# compose-clean:
# 	docker rmi -f booster_sim
