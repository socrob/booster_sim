all: help

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


# Build the Docker image
build:
	@docker build --tag booster_sim -f ./Dockerfile ..

# Remove the Docker image
clean:
	@docker rmi -f booster_sim

# Build with Docker Compose
compose-build:
	docker compose -f docker-compose.yml build --no-cache


compose-up:
	xhost +local:docker
	docker compose -f docker-compose.yml up -d

exec:
	docker compose -f docker-compose.yml exec booster_sim bash


compose-down:
	docker compose -f docker-compose.yml down


# Run with Docker Compose
# compose-up:
# 	xhost +local:docker
# 	docker compose -f docker-compose.yml run --rm booster_sim

# Clean image
# compose-clean:
# 	docker rmi -f booster_sim
