<script>
    import { browser, dev } from "$app/environment";
    import { onMount } from "svelte";

    let url = dev ? "http://localhost:5000" : "";
    if (!dev && browser) {
        url = location.protocol + "//" + location.host;
    }

    let downhill = 300;
    let uphill = 700;
    let length = 10000;

    let prediction = "n.a.";
    let linearPrediction = "n.a.";
    let din33466 = "n.a.";
    let sac = "n.a.";

    let debounceId;

    async function predict() {
        let result = await fetch(
            url +
                "/api/predict?" +
                new URLSearchParams({
                    downhill: downhill,
                    uphill: uphill,
                    length: length,
                }),
            {
                method: "GET",
            },
        );
        let data = await result.json();
        console.log(data);
        prediction = data.time;
        linearPrediction = data.linear;
        din33466 = data.din33466;
        sac = data.sac;
    }

    onMount(() => {
        predict();
    });

    function schedulePredict() {
        if (debounceId) {
            clearTimeout(debounceId);
        }
        debounceId = setTimeout(() => {
            predict();
        }, 300);
    }
</script>

<svelte:head>
    <title>HikePlanner benbehat</title>
    <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap-icons@1.11.1/font/bootstrap-icons.css">
</svelte:head>

<div class="app-bg d-flex align-items-center">
    <main class="container py-5">
        <div class="row g-4 align-items-stretch justify-content-center">
            
            <div class="col-lg-5">
                <div class="p-4 p-lg-5 bg-white shadow-modern rounded-4 h-100 d-flex flex-column">
                    <div class="mb-4">
                        <h1 class="display-6 fw-bold mb-2 text-primary">
                            <i class="bi bi-map"></i> HikePlanner
                        </h1>
                        <p class="text-muted">Schätze die Gehzeit basierend auf Distanz und Höhenmetern.</p>
                    </div>

                    <form class="vstack gap-4 flex-grow-1" on:submit|preventDefault={predict}>
                        <div class="input-group-modern">
                            <label class="form-label fw-semibold d-flex align-items-center">
                                <div class="icon-box bg-primary-subtle text-primary me-2">
                                    <i class="bi bi-graph-down-arrow"></i>
                                </div>
                                Abwärts [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg" bind:value={downhill} min="0" max="10000" on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={downhill} min="0" max="10000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="input-group-modern">
                            <label class="form-label fw-semibold d-flex align-items-center">
                                <div class="icon-box bg-danger-subtle text-danger me-2">
                                    <i class="bi bi-graph-up-arrow"></i>
                                </div>
                                Aufwärts [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg" bind:value={uphill} min="0" max="10000" on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={uphill} min="0" max="10000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="input-group-modern">
                            <label class="form-label fw-semibold d-flex align-items-center">
                                <div class="icon-box bg-success-subtle text-success me-2">
                                    <i class="bi bi-rulers"></i>
                                </div>
                                Distanz [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg" bind:value={length} min="0" max="30000" on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={length} min="0" max="30000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="d-grid mt-auto pt-4">
                            <button class="btn btn-primary btn-lg rounded-pill fw-semibold shadow-sm" type="submit">
                                <i class="bi bi-calculator me-1"></i> Berechnen
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <div class="col-lg-5">
                <div class="p-4 p-lg-5 bg-white shadow-modern rounded-4 h-100">
                    <div class="d-flex align-items-center mb-4">
                        <h2 class="h4 mb-0 fw-bold">Geschätzte Dauer</h2>
                        <i class="bi bi-stopwatch ms-auto fs-3 text-muted"></i>
                    </div>
                    
                    <div class="vstack gap-3">
                        <div class="result-card p-4 rounded-4 bg-primary text-white shadow-sm position-relative overflow-hidden">
                            <i class="bi bi-cpu position-absolute opacity-25" style="font-size: 6rem; right: -10px; top: -20px;"></i>
                            <div class="position-relative z-1">
                                <div class="text-white-50 fw-semibold mb-1 small text-uppercase tracking-wide">
                                    Hauptmodell (Gradient Boosting)
                                </div>
                                <div class="display-5 fw-bold">{prediction}</div>
                            </div>
                        </div>

                        <div class="row g-3 mt-1">
                            <div class="col-12">
                                <div class="p-3 rounded-3 border bg-light d-flex justify-content-between align-items-center">
                                    <div class="text-muted fw-medium"><i class="bi bi-graph-up text-secondary me-2"></i>Linear Regression</div>
                                    <div class="fw-bold fs-5">{linearPrediction}</div>
                                </div>
                            </div>
                            <div class="col-12">
                                <div class="p-3 rounded-3 border bg-light d-flex justify-content-between align-items-center">
                                    <div class="text-muted fw-medium"><i class="bi bi-signpost-split text-secondary me-2"></i>DIN33466</div>
                                    <div class="fw-bold fs-5">{din33466}</div>
                                </div>
                            </div>
                            <div class="col-12">
                                <div class="p-3 rounded-3 border bg-light d-flex justify-content-between align-items-center">
                                    <div class="text-muted fw-medium"><i class="bi bi-compass text-secondary me-2"></i>SAC</div>
                                    <div class="fw-bold fs-5">{sac}</div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </main>
</div>