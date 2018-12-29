### ---------------------------------------------------------------------------
### Target: hol-light-core
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-core

COPY --chown=opam:opam --from=holbox-ckpt /home/opam/hol-light/ckpt_core /home/opam/hol-light/dmtcp_restart_script*.sh /home/opam/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-multivariate
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-multivariate

COPY --chown=opam:opam --from=holbox-ckpt /home/opam/hol-light/ckpt_multivariate /home/opam/hol-light/dmtcp_restart_script*.sh /home/opam/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-complex
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-complex

COPY --chown=opam:opam --from=holbox-ckpt /home/opam/hol-light/ckpt_complex /home/opam/hol-light/dmtcp_restart_script*.sh /home/opam/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]

### ---------------------------------------------------------------------------
### Target: hol-light-hypercomplex
### ---------------------------------------------------------------------------

FROM hol-light-ckpt AS hol-light-hypercomplex

COPY --chown=opam:opam --from=holbox-ckpt /home/opam/hol-light/ckpt_hypercomplex /home/opam/hol-light/dmtcp_restart_script*.sh /home/opam/hol-light/

ENTRYPOINT ["./dmtcp_restart_script.sh"]
