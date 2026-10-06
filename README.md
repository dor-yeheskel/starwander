# Starwander

A relaxing, endless space-and-planet explorer in a single HTML file. Drift between galaxies, dive into star systems, land on procedurally generated worlds, and wander through forests, villages and great cities. Every world has its own creatures, plants, skies, landmarks and people you can stop and chat with.

Everything is generated on the fly from deterministic seeds with WebGL2 and WebAudio. There are no assets, no build step and no dependencies.

## Play

Just open: [https://starwander.app](https://starwander.app)

> For local play, open `index.html` in a recent desktop browser (Chrome, Edge or Firefox) and click to begin. You can also serve the folder with any static web server.

## Controls 

Press **?** in the game for the full controls. The basics:

| Where | Controls |
| --- | --- |
| Flying | **W** thrust · **Shift** boost · **Space** slow down · drag to steer · **Q / E** roll · **L** land |
| On foot | **W / S** walk · **A / D** turn · drag to steer · **Space** jump · **E** get in / out · **B** buggy · **F** light |
| People | **Click** to talk · **Hold** to scan (creatures and plants too) · **R** talk to the nearest person |
| Anywhere | **O** autopilot · **J** hyperspace jump to a selected target · **M** sound · **H** hide the interface |

## Talking to locals

People introduce themselves and talk about their home, the creatures, the peoples and the real landmarks and cities around them, with directions.

- **Offline:** replies are composed in the game from the world's own facts.
- **Online:** replies come from a small language model through a [Cloudflare Worker](worker/worker.js) that holds the API key. The game itself never contains a key. If the service can't be reached, the game falls back to the offline replies.

### Deploying your own chat Worker

1. Create a Cloudflare Worker and add an encrypted secret named `OPENAI_API_KEY`.
2. From the `worker` folder, run `npx wrangler deploy`. Or paste `worker.js` into the dashboard editor.
3. Point `CHAT_API` in `index.html` at your Worker's `/chat` URL.

The Worker fixes the model, the endpoint and the instructions. It validates and size-limits every request, rate-limits each player, and never passes raw errors back to the game. You can set `ALLOWED_ORIGINS` to restrict which sites may call it. Setting a monthly budget limit on the OpenAI project is recommended.
