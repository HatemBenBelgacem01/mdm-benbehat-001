<div class="app-bg">
    <main class="container py-5">
        <div class="row g-4 justify-content-center">
            
            <div class="col-lg-5">
                <div class="p-4 p-lg-5 glass-card h-100 d-flex flex-column">
                    <div class="mb-5">
                        <h1 class="h2 fw-bold mb-2 d-flex align-items-center">
                            <span class="text-primary me-2"><i class="bi bi-map-fill"></i></span>
                            HikePlanner
                        </h1>
                        <p class="text-muted small mb-0">Präzise Gehzeitschätzung durch Machine Learning.</p>
                    </div>

                    <form class="vstack gap-4 flex-grow-1" on:submit|preventDefault={predict}>
                        <div class="input-group-modern">
                            <label class="form-label small fw-bold text-uppercase text-muted mb-3 d-flex align-items-center">
                                <div class="icon-box bg-primary-subtle text-primary me-2">
                                    <i class="bi bi-graph-down-arrow"></i>
                                </div>
                                Abwärts [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg border-0 bg-light fw-bold" bind:value={downhill} on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={downhill} min="0" max="10000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="input-group-modern">
                            <label class="form-label small fw-bold text-uppercase text-muted mb-3 d-flex align-items-center">
                                <div class="icon-box bg-danger-subtle text-danger me-2">
                                    <i class="bi bi-graph-up-arrow"></i>
                                </div>
                                Aufwärts [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg border-0 bg-light fw-bold" bind:value={uphill} on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={uphill} min="0" max="10000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="input-group-modern">
                            <label class="form-label small fw-bold text-uppercase text-muted mb-3 d-flex align-items-center">
                                <div class="icon-box bg-success-subtle text-success me-2">
                                    <i class="bi bi-rulers"></i>
                                </div>
                                Distanz [m]
                            </label>
                            <div class="row g-3 align-items-center">
                                <div class="col-4">
                                    <input type="number" class="form-control form-control-lg border-0 bg-light fw-bold" bind:value={length} on:input={schedulePredict} />
                                </div>
                                <div class="col-8">
                                    <input type="range" class="form-range" bind:value={length} min="0" max="30000" step="10" on:input={schedulePredict} />
                                </div>
                            </div>
                        </div>

                        <div class="d-grid mt-auto pt-4">
                            <button class="btn btn-primary btn-lg rounded-pill fw-bold py-3 shadow-sm" type="submit">
                                Gehzeit berechnen
                            </button>
                        </div>
                    </form>
                </div>
            </div>

            <div class="col-lg-5">
                <div class="p-4 p-lg-5 glass-card h-100">
                    <div class="d-flex align-items-center mb-5">
                        <h2 class="h4 mb-0 fw-bold">Ergebnis</h2>
                        <span class="ms-auto badge bg-light text-muted border px-3 py-2 rounded-pill">
                            <i class="bi bi-stopwatch me-1"></i> Schätzung
                        </span>
                    </div>
                    
                    <div class="vstack gap-4">
                        <div class="result-card p-4 rounded-4 shadow-lg position-relative overflow-hidden">
                            <i class="bi bi-lightning-fill position-absolute opacity-10" style="font-size: 8rem; right: -20px; bottom: -30px;"></i>
                            <div class="position-relative z-1">
                                <div class="text-white-50 fw-bold small text-uppercase mb-1">
                                    KI-Vorhersage
                                </div>
                                <div class="display-4 fw-bold">{prediction}</div>
                            </div>
                        </div>

                        <div class="vstack gap-2">
                            <p class="small fw-bold text-uppercase text-muted px-1 mb-2">Vergleichswerte</p>
                            
                            <div class="comparison-item d-flex justify-content-between align-items-center">
                                <div class="text-muted small fw-medium">Linear Regression</div>
                                <div class="fw-bold">{linearPrediction}</div>
                            </div>
                            
                            <div class="comparison-item d-flex justify-content-between align-items-center">
                                <div class="text-muted small fw-medium">DIN 33466</div>
                                <div class="fw-bold">{din33466}</div>
                            </div>
                            
                            <div class="comparison-item d-flex justify-content-between align-items-center">
                                <div class="text-muted small fw-medium">SAC-Skala</div>
                                <div class="fw-bold">{sac}</div>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

        </div>
    </main>
</div>