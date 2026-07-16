# Self-contained build (runs the full bun build+sync INSIDE the Linux container,
# so no pre-built dist/ is required and Windows path limits are irrelevant).
FROM oven/bun:1.3.13-debian AS build
WORKDIR /app
COPY . .
RUN bun install
RUN bun run build
RUN cd packages/backend && bun run sync
RUN bun build --compile --minify --sourcemap ./packages/backend/src/bun.index.ts --outfile nordcraft-backend

# -------- RUN STAGE --------
FROM gcr.io/distroless/base-debian12 AS runner
ENV NODE_ENV=production
WORKDIR /app
COPY --from=build /app/nordcraft-backend .
COPY --from=build /app/packages/backend/dist/ .
ENTRYPOINT ["./nordcraft-backend"]
