#include <signal.h>
#include <stdio.h>
#include <string.h>
#include <unistd.h>
 
static int got_signal = 0;
 
static void hdl (int sig)
{
	got_signal = 1;
}
 
int main (int argc, char *argv[])
{
	sigset_t mask;
	sigset_t orig_mask;
	struct sigaction act;
 
	memset (&act, 0, sizeof(act));
	act.sa_handler = hdl;
 
	if (sigaction(SIGTERM, &act, 0)) {
		perror ("sigaction");
		return 1;
	}
 
	sigemptyset (&mask);
	sigaddset (
