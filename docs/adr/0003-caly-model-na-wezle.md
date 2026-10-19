# Cały model LLM na jednym węźle

Pula uruchamia Bielika (lub inny LLM) w całości na każdym węźle, a koordynator rozsyła zapytania do wolnych węzłów (jak AI Horde). Dzielenie modelu między maszyny (Petals, exo, llama.cpp RPC) odłożono, bo przez opóźnienia sieci działa wolno i ma sens tylko dla dużych modeli; Bielik 11B Q4 mieści się w ok. 12 GB VRAM.
