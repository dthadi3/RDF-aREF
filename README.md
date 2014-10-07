# NAME

RDF::aREF - Another RDF Encoding Form

# STATUS

[![Build Status](https://travis-ci.org/nichtich/RDF-aREF.png)](https://travis-ci.org/nichtich/RDF-aREF)
[![Coverage Status](https://coveralls.io/repos/nichtich/RDF-aREF/badge.png)](https://coveralls.io/r/nichtich/RDF-aREF)
[![Kwalitee Score](http://cpants.cpanauthors.org/dist/RDF-aREF.png)](http://cpants.cpanauthors.org/dist/RDF-aREF)

# SYNOPSIS

    use RDF::aREF;

    my $rdf = {
      _id       => 'http://example.com/people#alice',
      foaf_name => 'Alice Smith',
      foaf_age  => '42^xsd_integer',
      foaf_homepage => [
         { 
           _id => 'http://personal.example.org/',
           dct_modified => '2010-05-29^xsd_date',
         },
        'http://work.example.com/asmith/',
      ],
      foaf_knows => {
        dct_description => 'a nice guy@en',
      },
    };

    decode_aref( $rdf,
        callback => sub {
            my ($subject, $predicate, $object, $language, $datatype) = @_;
            ...
        }
    );
    
    my @lastmod = aref_query( $rdf, 'foaf_homepage.dct_modified^' );

    my $model = RDF::Trine::Model->new;
    decode_aref( $rdf, callback => $model );
    print RDF::Trine::Serializer->new('Turtle')->serialize_model_to_string($model);

# DESCRIPTION

**aREF** ([another RDF Encoding Form](http://gbv.github.io/aREF/)) is an
encoding of RDF graphs in form of arrays, hashes, and Unicode strings. This
module provides methods for decoding from aREF data to RDF triples
([RDF::aREF::Decoder](https://metacpan.org/pod/RDF::aREF::Decoder)), for encoding RDF data in aREF ([RDF::aREF::Encoder](https://metacpan.org/pod/RDF::aREF::Encoder)),
and for querying parts of an RDF graph ([RDF::aREF::Query](https://metacpan.org/pod/RDF::aREF::Query)).

# EXPORTED FUNCTIONS

The following functions are exported by default.

## decode\_aref( $aref, \[ %options \] )

Decodes an aREF document given as hash reference with [RDF::aREF::Decoder](https://metacpan.org/pod/RDF::aREF::Decoder).
Equivalent to `RDF::aREF::Decoder->new(%options)->decode($aref)`.

## aref\_query( $aref, \[ $origin \], $query )

Query parts of an aREF data structure. See [RDF::aREF::Query](https://metacpan.org/pod/RDF::aREF::Query) for details.

# SEE ALSO

- aREF is specified at [http://github.com/gbv/aREF](http://github.com/gbv/aREF).
- See [Catmandu::RDF](https://metacpan.org/pod/Catmandu::RDF) for an application of this module.
- Usee [RDF::Trine](https://metacpan.org/pod/RDF::Trine) for more elaborated handling of RDF data in Perl.
- See [RDF::YAML](https://metacpan.org/pod/RDF::YAML) for a similar (outdated) RDF encoding in YAML.

# COPYRIGHT AND LICENSE

Copyright Jakob Voss, 2014-

This library is free software; you can redistribute it and/or modify it under
the same terms as Perl itself.
