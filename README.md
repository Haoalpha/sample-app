#!/usr/bin/perl -w
use strict;
use warnings;
use LWP::UserAgent;

# Forces a flush after every write or print on the STDOUT
select STDOUT;
$| = 1;

# Create a UserAgent object
my $ua = LWP::UserAgent->new;

# Get the input line by line from the standard input.
# Each line contains a URL and some other information.
while (<>) {
    my @parts = split;
    my $url = $parts[0];

    if ( $url =~ /uti\.edu\.vn/ ) {
        # Make a request to the original URL
        my $response = $ua->get($url);

        if ($response->is_success) {
            my $content = $response->content;

            # Modify the content to replace image URLs
            $content =~ s{https://.*\.png}{https://i.imgur.com/q8axna8.png}g;

            # Print the modified content
            print $content;
        } else {
            # If the request fails, print an empty line
            print "\n";
        }
    } else {
        # No Rewriting.
        print "\n";
    }
}
