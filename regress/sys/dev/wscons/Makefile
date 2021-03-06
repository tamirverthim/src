# $OpenBSD: Makefile,v 1.8 2019/02/12 22:26:15 bluhm Exp $

WARNINGS=	yes

DEVICES=	wskbd0 wsmouse0 wskbd
.for d in ${DEVICES}
HAVE_${d} ?!= if dd if=/dev/${d} count=0 status=none; \
    then echo yes; else echo no; fi
MAKE := HAVE_${d}="${HAVE_${d}}" ${MAKE}
.endfor

PROG_sigio=	sigio
PROGS+=		${PROG_sigio}
SRCS_sigio=	sigio.c util.c

TESTS_sigio+=	getown-fcntl
TESTS_sigio+=	getown-ioctl
TESTS_sigio+=	gpgrp
TESTS_sigio+=	setown-fcntl
TESTS_sigio+=	setown-ioctl
TESTS_sigio+=	spgrp
TESTS_sigio+=	sigio

# SIGIO using event injection is limited to wsmux devices.
IGNORE_sigio+=	wskbd0-sigio
IGNORE_sigio+=	wsmouse0-sigio

.for d in ${DEVICES}
.  for t in ${TESTS_sigio}
.    if empty(IGNORE_sigio:M$d-$t)
REGRESS_TARGETS+=	${PROG_sigio}-$d-$t
${PROG_sigio}-$d-$t: ${PROG_sigio}
	@echo "\n======== ${@} ========"
.      if ${HAVE_${d}:L} == "yes"
	./${PROG_sigio} -d /dev/$d $t
.      else
	# no access to device ${d} on this machine
	@echo SKIPPED
.      endif
.    endif
.  endfor
.endfor

PROG_wscons=	wscons
PROGS+=		${PROG_wscons}
SRCS_wscons=	wscons.c util.c

TESTS_wscons+=	ioctl-unknown

.for d in ${DEVICES}
.  for t in ${TESTS_wscons}
REGRESS_TARGETS+=	${PROG_wscons}-$d-$t
${PROG_wscons}-$d-$t: ${PROG_wscons}
	@echo "\n======== ${@} ========"
.    if ${HAVE_${d}:L} == "yes"
	./${PROG_wscons} -d /dev/$d $t
.    else
	# device ${d} does not exists on this machine
	@echo SKIPPED
.    endif
.  endfor
.endfor

.include <bsd.regress.mk>
