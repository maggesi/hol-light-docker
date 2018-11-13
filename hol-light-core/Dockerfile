### ---------------------------------------------------------------------------
### Target: hol-light-core
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-core

COPY --chown=holuser:holuser --from=holbox-ckpt /home/holuser/hol-light/ckpt_core /home/holuser/hol-light/dmtcp_restart_script*.sh /home/holuser/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-multivariate
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-multivariate

COPY --chown=holuser:holuser --from=holbox-ckpt /home/holuser/hol-light/ckpt_multivariate /home/holuser/hol-light/dmtcp_restart_script*.sh /home/holuser/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-complex
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-complex

COPY --chown=holuser:holuser --from=holbox-ckpt /home/holuser/hol-light/ckpt_complex /home/holuser/hol-light/dmtcp_restart_script*.sh /home/holuser/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-hypercomplex
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-hypercomplex

COPY --chown=holuser:holuser --from=holbox-ckpt /home/holuser/hol-light/ckpt_hypercomplex /home/holuser/hol-light/dmtcp_restart_script*.sh /home/holuser/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]
