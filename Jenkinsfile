pipeline {
    agent any
    // In additional to manual runs, trigger somewhere at midnight to
    // give us the max time in a day to get things right.
    triggers {
	// Nightly @12am.
	cron('0 0 2-31 * *')
    }
    environment {
	///
	/// Automatic run variables.
	///

	// Pin dates and day to beginning of run.
	START_DATE = sh (
	    script: 'date +%Y-%m-%d',
	    returnStdout: true
	).trim()

	START_DAY = sh (
	    script: 'date +%A',
	    returnStdout: true
	).trim()

	///
	/// Internal run variables.
	///

	// The branch of XYZ to use.
	TARGET_XYZ_BRANCH = 'master'
	// The people to call when things go bad. It is a comma-space
	// "separated" string.
	TARGET_ADMIN_EMAILS = 'sjcarbon@lbl.gov'
	TARGET_SUCCESS_EMAILS = 'sjcarbon@lbl.gov,suzia@stanford.edu'
	// This variable should typically be 'TRUE', which will cause
	// some additional basic checks to be made. There are some
	// very exotic cases where these check may need to be skipped
	// for a run, in that case this variable is set to 'FALSE'.
	WE_ARE_BEING_SAFE_P = 'TRUE'
	// Control make to get through our loads faster if
	// possible. Assuming we're cpu bound for some of these...
	// wok has 48 "processors" over 12 "cores", so I have no idea;
	// let's go with conservative and see if we get an
	// improvement.
	MAKECMD = 'make --jobs --max-load 3.0'
	//MAKECMD = 'make'

	// Minerva operating profile.
	MINERVA_INPUT_ONTOLOGIES = [
	    "http://skyhook.berkeleybop.org/release/ontology/extensions/go-lego.owl"
	].join(" ")

	///
	/// GOlr/AmiGO input.
	///

	// GOlr load profile.
	GOLR_SOLR_MEMORY = "128G"
	GOLR_LOADER_MEMORY = "192G"
	GOLR_INPUT_ONTOLOGIES = [
	    "http://skyhook.berkeleybop.org/release/ontology/extensions/go-gaf.owl",
	    "http://skyhook.berkeleybop.org/release/ontology/extensions/gorel.owl",
	    "http://skyhook.berkeleybop.org/release/ontology/extensions/go-modules-annotations.owl",
	    "http://skyhook.berkeleybop.org/release/ontology/extensions/go-taxon-subsets.owl",
	    "http://purl.obolibrary.org/obo/eco/eco-basic.owl",
	    "http://purl.obolibrary.org/obo/ncbitaxon/subsets/taxslim.owl",
	    "http://purl.obolibrary.org/obo/cl/cl-basic.owl",
	    "http://purl.obolibrary.org/obo/pato.owl",
	    "http://purl.obolibrary.org/obo/po.owl",
	    "http://purl.obolibrary.org/obo/chebi.owl",
	    "http://purl.obolibrary.org/obo/uberon/basic.owl",
	    "http://purl.obolibrary.org/obo/wbbt.owl"
	].join(" ")
	GOLR_INPUT_GAFS = [
	    "http://skyhook.berkeleybop.org/release/products/annotations/paint_other.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/aspgd.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/cgd.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/dictybase.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/ecocyc.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/fb.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/genedb_lmajor.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/genedb_pfalciparum.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/genedb_tbrucei.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_chicken.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_chicken_complex.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_chicken_rna.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_cow.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_cow_complex.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_cow_rna.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_dog.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_dog_complex.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_dog_rna.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_human.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_human_complex.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_human_rna.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_pig.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_pig_complex.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_pig_rna.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/goa_uniprot_all_noiea.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/gramene_oryza.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/jcvi.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/mgi.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pamgo_atumefaciens.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pamgo_ddadantii.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pamgo_mgrisea.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pamgo_oomycetes.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pombase.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/pseudocap.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/rgd.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/sgd.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/sgn.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/tair.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/wb.gaf.gz",
	    "http://skyhook.berkeleybop.org/release/annotations/zfin.gaf.gz"
	].join(" ")
	GOLR_INPUT_PANTHER_TREES = [
	    "http://skyhook.berkeleybop.org/release/products/panther/arbre.tgz"
	].join(" ")

	///
	/// Groups to run and tests to avoid running during the current
	/// mega-make.
	///

	// The gorule tag is used to identify which rules to suppress
	// reports from during the megastep and during templating the
	// reports after the megastep. The tags are currently
	// respected at two times in the pipeline: the gorules report
	// take the flag as a CLI argument, supressing it; ontobio
	// takes it during the same stage as the JSON
	// generation/parsing step, to supress the .md output. At this
	// time, this variable can be either nothing or empty string
	// for no rule suppression (default behavior everything), or a
	// single value (practically speaking pretty much always
	// "silent")
	GORULE_TAGS_TO_SUPPRESS="silent"

	// Optional. Groups to run.
	//RESOURCE_GROUPS=""
	// Optional. Datasets to skip within the resources that we
	// will run (defined in the line above).
	//DATASET_EXCLUDES=""
	// Optional. This acts as an override, /if/ it's grabbed (as
	// defined above).
	//GOA_UNIPROT_ALL_URL=""
    }
    options{
	timestamps()
	buildDiscarder(logRotator(numToKeepStr: '14'))
    }
    stages {
	// Very first: pause for a few minutes to give a chance to
	// cancel and clean the workspace before use.
	stage('Ready and clean') {
	    steps {

		// Check that we do not affect public targets on
		// non-mainline runs.
		script {
		    if( BRANCH_NAME != 'master'){
			echo 'Only master can touch that target.'
			sh '`exit -1`'
		    }else if( BRANCH_NAME != 'snapshot'){
			echo 'Only master can touch that target.'
			sh '`exit -1`'
		    }else if( BRANCH_NAME != 'release'){
			echo 'Only master can touch that target.'
			sh '`exit -1`'
		    }
		}

		// Give us a minute to cancel if we want.
		sleep time: 1, unit: 'MINUTES'
		cleanWs()
	    }
	}
	stage('Initialize') {
	    steps {
		// Start preparing environment.
		parallel(
		    "Report": {
			sh 'env > env.txt'
			sh 'echo $BRANCH_NAME > branch.txt'
			sh 'echo "$BRANCH_NAME"'
			sh 'cat env.txt'
			sh 'cat branch.txt'
			sh 'echo $START_DAY > dow.txt'
			sh 'echo "$START_DAY"'
		    },
		    "Reset base": {
			// Get a mount point ready
			sh 'mkdir -p $WORKSPACE/mnt || true'
		    }
		)
	    }
	}
	stage('Produce ontology') {
            agent {
                docker {
		    image 'obolibrary/odkfull:v1.1.7'
		    // Reset Jenkins Docker agent default to original
		    // root.
		    args '-u root:root'
		}
            }
	    steps {
		// Create a relative working directory and setup our
		// data environment.
		dir('./mammalian-phenotype-ontology') {
		    git 'https://github.com/obophenotype/mammalian-phenotype-ontology.git'

		    // Default namespace.
		    sh 'OBO=http://purl.obolibrary.org/obo'

		    dir('./src/ontology') {
			retry(3){
			    sh 'sh run.sh make all'
			}
		    }

		    // // Make sure that we copy any files there,
		    // // including the core dump of produced.
		    // withCredentials([file(credentialsId: 'skyhook-private-key', variable: 'SKYHOOK_IDENTITY')]) {
		    // 	//sh 'rsync -avz -e "ssh -o StrictHostKeyChecking=no -o IdentitiesOnly=true -o IdentityFile=$SKYHOOK_IDENTITY" target/* skyhook@skyhook.berkeleybop.org:/home/skyhook/$BRANCH_NAME/ontology'
		    // 	sh 'scp -o StrictHostKeyChecking=no -o IdentitiesOnly=true -o IdentityFile=$SKYHOOK_IDENTITY -r target/* skyhook@skyhook.berkeleybop.org:/home/skyhook/$BRANCH_NAME/ontology/'
		    // }

		    // Now that the files are safely away onto skyhook for
		    // debugging, test for the core dump.
		    script {
			if( WE_ARE_BEING_SAFE_P == 'TRUE' ){

			    def found_core_dump_p = fileExists 'target/core_dump.owl'
			    if( found_core_dump_p ){
				error 'ROBOT core dump detected--bailing out.'
			    }
			}
		    }
		}
	    }
	}
	stage('Archive') {
	    when { anyOf { branch 'release'; branch 'snapshot'; branch 'master' } }
	    steps {
		// Experimental stanza to support mounting the sshfs
		// using the "hidden" skyhook identity.
		sh 'mkdir -p $WORKSPACE/mnt/ || true'
	    }
	}
    }
    post {
	// Let's let our people know if things go well.
	success {
	    script {
		if( env.BRANCH_NAME == 'release' ){
		    echo "There has been a successful run of the ${env.BRANCH_NAME} pipeline."
		}
	    }
	}
	// Let's let our internal people know if things change.
        changed {
	    echo "There has been a change in the ${env.BRANCH_NAME} pipeline."
	}
	// Let's let our internal people know if things go badly.
	failure {
	    echo "There has been a failure in the ${env.BRANCH_NAME} pipeline."
	    mail bcc: '', body: "There has been a pipeline failure in ${env.BRANCH_NAME}. Please see: https://build.obolibrary.io/job/OBOFoundry/job/pipeline-mp/job/${env.BRANCH_NAME}", cc: '', from: '', replyTo: '', subject: "MP Pipeline FAIL for ${env.BRANCH_NAME}", to: "${TARGET_ADMIN_EMAILS}"
        }
    }
}
